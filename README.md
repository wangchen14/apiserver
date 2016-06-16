controller


<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

use App\Http\Requests;

use DB;

use Validator;
use Illuminate\Database\Eloquent\Model;

class StudentController extends Controller
{
    
 

	public function __construct(){

		}

	public function showRegisterForm(){

		return view('register');


	}




	public function register(Request $request){



            $credentials = $request->all();
           
            $validator = Validator::make($request->all(), [


             // 'mobile' => 'required|unique |max:255',
            'email' => 'required | email',
            'password'=> 'required'
                ]);


            if ($validator->fails()) {

            	$status = array(
            		"status"=>"false",
            		 "message"=>"you validation failed"

            		 );

                   

                return json_encode($status);

                }



              $token= rand();

            $user = new \App\User ;

          
            $user->email= $request->email;


            $user->password=$request->password;

             $user->remember_token = $token;


            $user->save();

					$status = array(
						"status"=>'true',
            		
            		 "message"=>"login Succesfull!!",

            		 "token"=>$token,

            		 );
 			return json_encode($status);



	}


	public function showLoginForm(){
			
				return view('login');

	}


public function login(Request $request){


		   $credentials = $request->all();

		    $validator = Validator::make($request->all(), [
				// 'mobile' => 'required|unique |max:255',
	            'email' => 'required | email',
	            'password'=> 'required'
	              
	                ]);


            if ($validator->fails()) {

            	$status = array(
            		"status"=>"false",
            		 "message"=>"your validation failed" 
            		 );
               		return json_encode($status);

                }


                $email= $request->email;
                $password= $request->password;

                $DBemail = DB::table('users')->where('email',$email)->pluck('email');
                $DBpassword = DB::table('users')->where('password',$password)->pluck('password');

                $stremail = implode(" ",$DBemail);
                $strpassword = implode(" ",$DBpassword);

                 


                if( $email == $stremail && $password == $strpassword){
         		 
         		$status = array(
            		"status"=>"true",
            		 "message"=>"success" 
            		 );

               		return json_encode($status);
                
                }

                else{

                	$status = array(
            		"status"=>"false",
            		 "message"=>"email or password is wrong !!" 
            		 );

               		return json_encode($status);

                }





}


}



migration 

<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateUsersTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->increments('id');
            $table->string('name');
            $table->integer('mobile')->unsigned();

            $table->string('email')->unique();
            $table->string('password');
            $table->rememberToken();
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::drop('users');
    }
}


view 

<!DOCTYPE html>
<html>
<body>
@extends('form_layouts._layouts')

@section('content')
  <br>  <br> 
  <div class="col-md-offset-1">
  <h3>the form from the mobile</h3>
  <br>

  {{ Form::open(array('url' => 'api/login', 'method'=>'post')) }}


   
	   
	   {{Form::label('email ', 'Email :')}}
	   {{Form::Text('email')}}
   		<br>



	 {{Form::label('password ', 'Password :')}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
    {{Form::Text('password')}}

   <!--  {{ Form::hidden('authToken', '123') }} -->
  

     <br><br>

    {{Form::submit('Submit')}}
    



	{!! Form::close() !!}

	</div>

	

@stop
</body>
</html>



<!DOCTYPE html>
<html>
<body>
@extends('form_layouts._layouts')

@section('content')
  <br>  <br> 
  <div class="col-md-offset-1">
  <h3>the form from the mobile</h3>
  <br>

  {{ Form::open(array('url' => 'api/register', 'method'=>'post')) }}


   {{Form::label('mobile ', 'Mobile :')}}
     {{Form::Text('mobile')}}

     <br>
	   
	   {{Form::label('email ', 'Email :')}}
	   {{Form::Text('email')}}
   		<br>



	 {{Form::label('password ', 'Password :')}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
    {{Form::Text('password')}}

   <!--  {{ Form::hidden('authToken', '123') }} -->
  

     <br><br>

    {{Form::submit('Submit')}}
    



	{!! Form::close() !!}

	</div>

	

@stop
</body>
</html>


layout

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>Laravel</title>

    <!-- Fonts -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.5.0/css/font-awesome.min.css" integrity="sha384-XdYbMnZ/QjLh6iI4ogqCTaIjrFk87ip+ekIjefZch0Y+PvJ8CDYtEs1ipDmPorQ+" crossorigin="anonymous">
    <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Lato:100,300,400,700">

    <!-- Styles -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.6/css/bootstrap.min.css" integrity="sha384-1q8mTJOASx8j1Au+a5WDVnPi2lkFfwwEAa8hDDdjZlpLegxhjVME1fgjWPGmkzs7" crossorigin="anonymous">
    {{-- <link href="{{ elixir('css/app.css') }}" rel="stylesheet"> --}}

    <style>
        body {
            font-family: 'Lato';
        }

        .fa-btn {
            margin-right: 6px;
        }
    </style>
</head>
<body id="app-layout">
    <nav class="navbar navbar-default navbar-static-top">
        <div class="container">
            <div class="navbar-header">

                <!-- Collapsed Hamburger -->
                <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#app-navbar-collapse">
                    <span class="sr-only">Toggle Navigation</span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>

                <!-- Branding Image -->
                <a class="navbar-brand" href="{{ url('/') }}">
                    Student deatails
                </a>
            </div>

            <div class="collapse navbar-collapse" id="app-navbar-collapse">
                <!-- Left Side Of Navbar -->
                <ul class="nav navbar-nav">
                    <li><a href="{{ url('/home') }}">Home</a></li>
                </ul>

                <!-- Right Side Of Navbar -->
                <ul class="nav navbar-nav navbar-right">
                    <!-- Authentication Links -->
                    @if (Auth::guest())
                        <li><a href="{{ url('/login') }}">Login</a></li>
                        <li><a href="{{ url('/register') }}">Register</a></li>
                    @else
                        <li class="dropdown">
                            <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false">
                                {{ Auth::user()->name }} <span class="caret"></span>
                            </a>

                            <ul class="dropdown-menu" role="menu">
                                <li><a href="{{ url('/logout') }}"><i class="fa fa-btn fa-sign-out"></i>Logout</a></li>
                            </ul>
                        </li>
                    @endif
                </ul>
            </div>
        </div>
    </nav>

    @yield('content')

    <!-- JavaScripts -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.2.3/jquery.min.js" integrity="sha384-I6F5OKECLVtK/BL+8iSLDEHowSAfUo76ZL9+kGAgTRdiByINKJaqTPH/QVNS1VDb" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.6/js/bootstrap.min.js" integrity="sha384-0mSbJDEHialfmuBBQP6A4Qrprq5OVfW37PRR3j5ELqxss1yVqOtnepnHVP9aJ7xS" crossorigin="anonymous"></script>
    {{-- <script src="{{ elixir('js/app.js') }}"></script> --}}
</body>
</html>

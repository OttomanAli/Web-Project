# Web-Project
############CSS-file##########(style.css)
#main{
	text-align: center;
	font-family: cursive;
	margin-left: ;
}
#maintable{
	margin-left: 41% ;
	border: solid 2px black;
	border-radius:5px;

}
td{
	//border: solid 2px black;
	margin: none;
	padding: ;
}

#name, #password{
	text-align: left;

}
.label{
	text-align: right;
}
tr #none{
	border: none;
}
.error{
	font-weight: bold;
	color: red;
}



.profile{
	text-align: center;
}
#profiletable{
	margin-left: 37% ;


}
thead tr{
	font-weight: bold;
}

.p_td{ 
	border: solid 2px black;
	border-radius: 10px;
	margin-left: 25px;
	padding:;
}


#signup{
	text-align: center;
}
#signuptable{
	margin-left: 40%;
	text-align: left;
}

#######################################
##############Login.php################
<?php
	include("loginphp.php");

	if(isset($_SESSION["login_user"])){
		header('Location: profile.php');
		die();
}
?>

<!DOCTYPE html>
<html>
<head>
	<title>Login </title>
	<link href="style.css" rel="stylesheet" type="text/css">
</head>
<body>
	<div id="main">
		<h1>Please Login</h1>
		<div id="login">
			<h2>Login Form</h2>
			<table id="maintable">
				<form action="" method="post">
					<tr>
						<td class="label"><label>UserName :</label></td>
						<td><input id="name" name="username" placeholder="username" type="text" /><br /></td>
					</tr>
					<tr>
						<td class="label"><label>Password :</label></td>
						<td><input id="password" name="password" placeholder="**********" type="password" /></td>
					</tr>
					<tr>
						<td id="none"></td>
						<td>
							<input name="submit" type="submit" value=" Login " />
							<input type="reset" value=" Reset " /> 
						</td>
					</tr>
					
				<span class="error"><?php echo $error; ?></span>
				</form>
			</table>
			<p>If new user then Sign Up here!</p>
			<a href="signup.php"><button>Sign Up</button></a>
		</div>
	</div>
</body>
</html>

#########################################
###############loginphp.php##################
<?php

	session_start();
	$error = "";

	if(isset($_POST["submit"]))
	{
		
		if(empty($_POST["username"]) || empty($_POST["password"])) 
		{
			$error = "Fill All Fields!";	
		}
		else 
		{
			$username = $_POST["username"]; 
			$password = $_POST["password"];
		

			//$up = $username." ".$password;	
			
			if(!mysql_connect('localhost','root','')||!mysql_select_db('store')){
				die('Could Not Connect');	
			}

			$q = "SELECT * FROM login WHERE userID = '$username'";
			$query = mysql_query($q);

			if($query){ 
				if(mysql_num_rows($query)>0){

					while($row=mysql_fetch_array($query)){
						$Name=$row['userID'];
						$pas=$row['password'];
						$rank=$row['rank'];
					}
					$find = false;
					if($Name == $username && $pas == $password){
						$_SESSION["login_user"] = $username;
					}
					else{
						$error = "Invalid Username or Pasword!";
					}

				}
				else{
					$error = "User Doesn't Exist!\n Please Sign Up!";
				}
			}
			else{
				$error = "Query Didn't Run!";
			}

			/*$filename = "logins.txt";
			$myfile = fopen($filename, "r") or die("Couldn't Open!");
			$arr = array("");
			$c = '0';
			while(!feof($myfile)){
				$arr[$c] = fgets($myfile);   
				//echo "<br>";
				$arr[$c] = trim($arr[$c],PHP_EOL);
				$c++;
			}
			
			fclose($myfile);*/
			
			

		

			

			/*for ($cc='0'; $cc<$c ; $cc++) { 
				$com = strcmp($arr[$cc], $up);
				if ($com == '0'){
					$find = true;
					break;
				}
			}
*/			
			
		}
	}
	
?>

##########################################
################profile.php###############
<?
session_start();
if(!isset($_SESSION["login_user"]))
{
	header("Location: login.php");
	exit();
}
?>
<!DOCTYPE html>
<html>
<head>
<title>Home Page</title>
	
	<link href="style.css" rel="stylesheet" type="text/css">

</head>
<body>
<div class="profile">

	<p><b id="welcome">Welcome : <i><?php echo $_SESSION["login_user"]; ?></i> <br />
	Here Is Your Profie!!</b></p>

	<?php
		if(!mysql_connect('localhost','root','')||!mysql_select_db('store')){
					die('Could Not Connect');	
		}
		$username = $_SESSION["login_user"];
			$q = "SELECT * FROM profiles WHERE uID = '$username'";
			$query = mysql_query($q);
			
			$row = mysql_fetch_array($query);

				$userid = $row['uID'];
				$fname = $row['fName'];
				$gender = $row['gender'];
				$age = $row['age'];
				$city = $row['city'];
				$state = $row['state'];

	?>
		<table id="profiletable">
			<thead>
				<tr>
					<td class="p_td">User ID</td>
		            <td class="p_td">Full Name</td>
		            <td class="p_td">Gender</td>
		            <td class="p_td">Age</td>
		            <td class="p_td">City</td>
		            <td class="p_td">State</td>
				</tr>
			</thead>
		<?php	
			echo "
	        <tr>
	            <td class=\"p_td\">$username</td>
	            <td class=\"p_td\">$fname</td>
	            <td class=\"p_td\">$gender</td>
	            <td class=\"p_td\">$age</td>
	            <td class=\"p_td\">$city</td>
	            <td class=\"p_td\">$state</td>
	        </tr>
	        ";
	    ?>
		</table>
	<!-- <input type="text" name="fname" value='$fname'  /> -->
	<a href="logout.php"><button id="logout">Log Out</button></a>
</div>
</body>
</html>

##########################################
################signup.php################

<?php
	include("signupphp.php");


?>


<!DOCTYPE html>
<html>
<head>
	<title>Sign Up </title>
	<link href="style.css" rel="stylesheet" type="text/css">
	<script >
		/*$.function(){
			$("button").click(function(){

			});
		}		*/
	</script>
</head>
<body>
	<div id="">
		<h1>Please enter your Detail</h1>
		<div id="signup">
			<h2>Sign Up Form</h2>
			<p>All Fields are Mendatory! </p>
			<form action="" method="post">
				<table id="signuptable">
					<tr>
						<td class="label"><label>Full Name :</label></td>
						<td><input id="name" name="name" placeholder="fullname" type="text"><br /></td>
					</tr>
					<tr>
						<td class="label"><label>UserName :</span></td>
						<td><input id="name" name="username" placeholder="username" type="text"><br /></td>
					
						<span class="error"><?php echo $usererror?></span>
					</tr>
					<tr>
						<td class="label"><label>Password :</label></td>
						<td><input id="password" name="password" placeholder="**********" type="password"><br /></td>
					</tr>
					<tr>
						<td class="label"><label>Age :</label></td>
						<td><input id="age" name="age" placeholder="age" type="text"><br /></td>
					</tr>
					<tr>
						<td class="label"><label>Gender:</label><br /></td>
						<td><input type="radio" name="gender" value="Male" checked="	checked" />
						<label>Male</label></td>
					</tr>
					<tr>
						<td></td>
						<td><input type="radio" name="gender" value="Female"  />
						<label>Female</label><br /></td>
					</tr>
					<tr>
						<td class="label"><label>City :</label></td>
						<td><input id="city" name="city" placeholder="city" type="text"><br /></td>
					</tr>
					<tr>
						<td class="label"><label>Contry :</label></td>
						<td><input id="state" name="state" placeholder="state" type="text"><br /></td>
					</tr>
					<tr>
						<td></td>
						<td><input name="submit" type="submit" value=" Sign Up! ">
							<input type="reset" value="Reset"></td>
					</tr>
				<span class="error"><?php echo $error; ?></span>
			</table>
			</form>
		
		</div>
	</div>
</body>
</html>

#############################################
#################signupphp.php###############
<?php
	
	$error = "";
	$usererror = "";

	if(isset($_POST["submit"]))
	{
		
		if( empty($_POST["name"]) || empty($_POST["username"]) || empty($_POST["password"]) || empty($_POST["age"]) || empty($_POST["gender"]) || empty($_POST["city"]) || empty($_POST["state"])) 
		{
			$error = "Fill All Fields!";	
		}
		else 
		{
			$fname = $_POST["name"]; 
			$username = $_POST["username"]; 
			$password = $_POST["password"];
			$age = $_POST["age"];
			$gender = $_POST["gender"];
			$city = $_POST["city"];
			$state = $_POST["state"];
			

			
			
			if(!mysql_connect('localhost','root','')||!mysql_select_db('store')){
				die('Could Not Connect');	
			}

			$search = "SELECT * FROM login WHERE userID = '$username'";
			$qSearch = mysql_query($search); 

			if(mysql_num_rows($qSearch)==0){

				$q = "INSERT INTO login (userID, password, rank)VALUES ('$username', '$password', 'user')";
				$query = mysql_query($q);

				
				$q1 = "INSERT INTO profiles (uid, fName, gender, age, city, state)VALUES ( '$username', '$fname' , '$gender', '$age', '$city', '$state')";
				$query1 = mysql_query($q1);

				/*if ($query1) {
						$error = "Not Fucked";
					}*/
				if($query && $query1){ 
					$error = "Enter Successfully!!";
					session_start();
					$_SESSION["login_user"] = $username;
						header('Location: profile.php');
						die();

					/*if(mysql_num_rows($query)>0){

						while($row=mysql_fetch_array($query)){
							$Name=$row['userID'];
							$pas=$row['password'];
							$rank=$row['rank'];
						}
						$find = false;
						if($Name == $username && $pas == $password){
							$_SESSION["login_user"] = $username;
						}
						else{
							$error = "Invalid Username or Pasword!";
						}

					}
					else{
						$error = "User Doesn't Exist!\n Please Sign Up!";
					}*/
				}
				else{
					$error = "Query Didn't Run!";
				} 
			}
			else {
				$error = "Please Select Another User Name";
				$usererror = "User Already Exist <br >";
			}

			/*$filename = "logins.txt";
			$myfile = fopen($filename, "r") or die("Couldn't Open!");
			$arr = array("");
			$c = '0';
			while(!feof($myfile)){
				$arr[$c] = fgets($myfile);   
				//echo "<br>";
				$arr[$c] = trim($arr[$c],PHP_EOL);
				$c++;
			}
			
			fclose($myfile);*/
			
			

		

			

			/*for ($cc='0'; $cc<$c ; $cc++) { 
				$com = strcmp($arr[$cc], $up);
				if ($com == '0'){
					$find = true;
					break;
				}
			}
*/			
			
		}
	}
	
?>

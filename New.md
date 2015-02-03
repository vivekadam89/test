<?php session_start(); ?>
<?php include('connectivity.php'); ?>
<!doctype html>
<html>
<head>
	<script type="text/javascript" src="js/jquery.js"></script>
	<script type="text/javascript">
		function getCity(str)
		{
			var state = str;
			$.ajax({
				type : "GET",
				url : "getdata.php",
				data : {state : state}
			}).done(function(data){
				$("#txthint").html(data);
			});
		}
		$(document).ready(function(){
			var state = $("#ddl_state").val();
			getCity(state);
		});
	</script>
	
	<style type="text/css">
		.error{
			color:#FF0000;
			font-size:10px;
		}
	</style>
</head>
<body>
	<?php 
	if(isset($_POST["btn_submit"]))
	{
	  $error = "";
	  if(empty($_POST["fullname"]))
	  {
	  	$error = "Please Enter full name";
	  }
	  else if(!preg_match('/^[a-zA-Z]+\s?[a-zA-Z]+$/',$_POST["fullname"]))
	  {
		$error = "Name is not valid";
	  }
	  else if(empty($_POST["mobilenumber"]))
	  {
	  $error = "Please Enter Mobile number" ;
	  }
	  else if(!is_numeric($_POST["mobilenumber"]))     //not null mobile number
	  {
	  $error = "Please provide valid number";
	  }
	  else if(strlen($_POST["mobilenumber"]) !==11)    // mobile length
	  {
	  $error = "Please provide 11 character number";
	  }
	  else if(empty($_POST["emailid"]))
	  {
	  $error = "Please Enter Email-id";
	  }
	  else if(!filter_var(test_input($_POST["emailid"]), FILTER_VALIDATE_EMAIL)) //email format
	  {
	  $error = "Please provide valid Email format.";
	  }
	  else if(empty($_POST["password"]))
	  {
	  $error = "Please Enter password";
	  }
	  else if(strlen($_POST["password"]) <=5)
	  {
	  $error = "Please provide atleast 6 length password ";
	  }
	  else if(empty($_POST["password"]))
	  {
	  $error = "Please retype password";
	  }
	  else if($_POST["retypepassword"]!==$_POST['password'])
	  {
	  $error = "Please check password";
	  }
		else if(isset($_POST["captcha"])&&$_POST["captcha"]!=""&&$_SESSION["code"]==$_POST["captcha"])
		{
			$error = "Correct Code Entered";
		}
		else
		{
			$error = "Wrong Code Entered";
		}
	}
	?>
<form name= "signup" id= "signup" method= "post">
	<table width="500" border="0" align="center" cellspacing="0" cellpadding="8">
	  <tr>
		<td colspan="3"><font color="#FF0000">*All Fields are mandatory.</font></td>
	  </tr>
	  <tr>
		<td width="500"><font color="#FF0000">*</font>Full Name</td>
		<td>:</td>
		<td width="900"><input type="text" name="fullname" value="" id=""/><span class="error"><?php if(isset($error)){echo $error;}?></span></td>
	  </tr>
	  <tr>
		<td width="500"><font color="#FF0000">*</font>Location</td>
		<td>:</td>
		<td width="900">State : <select name="ddl_state" id="ddl_state" onChange="getCity(this.value)">
		<?php
		$sql="SELECT * FROM state";
		$result = mysql_query($sql);
		while($row = mysql_fetch_array($result))
		  {
		  $id=$row['id'];
		  echo "<option value='$id'>" . $row['state'] . "</option>";
		}
		?>
		</select>
		<br />
			<div id="txthint">
				
			</div>
		</td>
	  </tr>
	  <tr>
		<td width="500"><font color="#FF0000">*</font>Mobile Number</td>
		<td>:</td>
		<td width="900"><input type="text" name="mobilenumber" value="" id=""/><span class="error"><?php if(isset($error)){echo $error;}?></span></td>
	  </tr>
	  <tr>
		<td width="500"><font color="#FF0000">*</font>Email-Id</td>
		<td>:</td>
		<td width="900"><input type="text" name="emailid" value="" id=""/><span class="error"><?php if(isset($error)){echo $error;}?></span></td>
	  </tr>
	  <tr>
		<td width="500"><font color="#FF0000">*</font>Password</td>
		<td>:</td>
		<td width="900"><input type="password" name="password" value="" id=""/><span class="error"><?php if(isset($error)){echo $error;}?></span></td>
	  </tr>
	  <tr>
		<td width="500"><font color="#FF0000">*</font>Re-type Password</td>
		<td>:</td>
		<td width="900"><input type="password" name="retypepassword" value="" id=""/><span class="error"><?php if(isset($error)){echo $error;}?></span></td>
	  </tr>
	  <tr>
		<td width="500"><font color="#FF0000">*</font>Enter the code shown below</td>
		<td>:</td>
		<td width="900"><input name="captcha" type="text"><span class="error"><span class="error"><?php if(isset($error)){echo $error;}?></span></span></td>
	  </tr>
	  <tr>
		<td width="500"></td>
		<td>:</td>
		<td width="900"><img src="captcha.php" /></td>
	  </tr>
	  <tr>
		<td width="500"></td>
		<td></td>
		<td width="900"></td>
	  </tr>
	  <tr>
	  	<td colspan="3" align="center"><input type="submit" name="btn_submit" id="btn_submit" value="Login" />
		<input type="submit" name="btn_reset" id="btn_reset" value="Reset" /></td>
	  </tr>
	</table>
</form>
</body>
<?php /*?><?php 
if(isset($_POST['btn_submit']))
{
$_POST['fullname'];
$_POST[''];  //state value
$_POST[''];  //city value
$_POST['mobilenumber'];
$_POST['emailid'];
$_POST['password'];
$_POST['retypepassword'];
$_POST[''];
}
?><?php */?>
</html>


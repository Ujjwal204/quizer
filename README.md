# quizer
w3villa assinment
// quiz.php code below



// there are 7 codes each seperated by new lines









//quiz.php code or index

<?php include 'database.php'; ?>
<?php

$query ="SELECT *FROM questions";

$results = $mysqli->query($query) or die($mysqli->error.__LINE__);
$total = $results->num_rows;
?>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8" />
<title>Quiz</title>
<link rel="stylesheet" href="css/style.css" type="text/css" />
</head>
<body>
    <header>
        <div class="container">
            <h1>Quiz</h1>
        </div>
    </header>
    <main>
        <div class="container">
            <h2>Test Your General Knowledge</h2>
            <p>This is a multiple choice quiz to test your general knowledge<p>
            <u1>
                <li><strong>Number of Questions: </strong>5</li>
                <li><strong>mType: </strong>ultiple choice</li>
                <li><strong>Estimated time: </strong>30sec</li>
            </u1>
            <a href="question.php?n=1" class="start">Start Quiz</a>
        </div>
    </main>
    <footer>
        <div class="container">
            Copyright &copy; 2014, Quiz
        </div>
    </footer>
</body>
</html>










//question.php code
<?php include 'database.php'; ?>
<?php
    //setting ques
    $number=(int) $_GET['n'];
    
    $query="SELECT * FROM 'questions'
                WHERE question_number = $number";
    $result = $mysqli->query($query) or die($mysqli->error.__LINE__);

    $question = $result->fetch_assoc();

    //new
    $query="SELECT * FROM 'choices'
                WHERE question_number = $number";
    $choices = $mysqli->query($query) or die($mysqli->error.__LINE__);
?>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8" />
<title>Quiz</title>
<link rel="stylesheet" href="css/style.css" type="text/css" />
</head>
<body>
    <header>
        <div class="container">
            <h1>Quiz</h1>
        </div>
    </header>
    <main>
        <div class="container">
            <div class="current">Question 1 to 5</div>
            <p class="question">
                <?php echo $qestion['text']; ?>
            </p>
            <form method="post" action="process.php">
                <u1 class="choice">
                <?php while($row = $choices->fetch_assoc()): ?>
                <li><input name="choice" type="radio" value="<?php echo$row['id']; ?>" /><?php echo $row['text']; ?></li>
                <?php endwhile; ?>
                </u1>
                <input type="submit" value="Submit" />
                <input type="hidden" name="number" value="?php echo $number; ?>" />
            </form>
        </div>
    </main>
    <footer>
        <div class="container">
            Copyright &copy; 2014, Quiz
        </div>
    </footer>
</body>















//style.css code

body{
    font-family:arial;
    font-size:15px;
    line-height:1.6em;
}

li{
    list-style:none;
}

a{
    text-decoration:none;
}

.container{
    width:60%;
    margin:0 auto;
    overflow:auto;
}

header{
    border-bottom:3px #f4f4f4 solid;
}

footer{
    border-top:3px #f4f4f4 solid;
    text-align:center;
    padding-top:5px;
}

main{
    padding-bottom:20px;
}

a.strat{
    display: inline-block;
    color:#666;
    background:#f4f4f4;
    border:1px dotted #ccc;
    padding:6px 13px;
}

.current{
    padding:10px;
    background:#f4f4f4;
    border:#ccc dotted 1px;
    margin:20px 0 10px 0;
}










//result.php code

<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8" />
<title>Quiz</title>
<link rel="stylesheet" href="css/style.css" type="text/css" />
</head>
<body>
    <header>
        <div class="container">
            <h1>Quiz</h1>
        </div>
    </header>
    <main>
        <div class="container">
            <h2>You're Done</h2>
                <p>your grade is</p>
                <p>Final Score: 5</p>
                <a href="question.php?n=1" class="start">Take Again</a>
        </div>
    </main>
    <footer>
        <div class="container">
            Copyright &copy; 2014, Quiz
        </div>
    </footer>
</body>
</html>










//add.php code

<?php include 'database.php'; ?>
<?php
    if(isset($_POST['submit'])){
        $question_number = $_POST['qestion_number'];
        $choices = int();
        $choices[1]= $_POST['Hadr'];
        $choices[2]= $_POST['Medium'];
        $choices[3]= $_POST['Easy'];

        $query = "INSERT INTO 'questions' (question_number, text)
                    VALUE('$question_number')";

        $insert_row = $mysqli->query($query) or die($mysqli->error.__LINE__);
        
    }
?>

<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8" />
<title>Quiz</title>
<link rel="stylesheet" href="css/style.css" type="text/css" />
</head>
<body>
    <header>
        <div class="container">
            <h1>Quiz</h1>
        </div>
    </header>
    <main>
        <div class="container">
            <h2>Chosee your level: </h2>
            <from method="post" action="add.php">
                <p>
                    <lable>1-Hard, 2-Medium, 3-Easy</lable>
                    <input type="number" name="number" />
                </p>
                <p>
                    <input type="submit" name="submit" value="Submit" />
                </p>
            </from>
        </div>
    </main>
    <footer>
        <div class="container">
            Copyright &copy; 2014, Quiz
        </div>
    </footer>
</body>
</html>










//process.php code

<?php include 'database.php'; ?>
<?php session_start(); ?>
<?php

    if(!isset($_SESSION['score'])){
        $_SESSION['score']=0;

    }
    if($_POST){
        $number = $_POST['number'];
        $selected_choice=$_POST['choice'];
        $next = $number+1;

        $query = "SELECT * FROM 'QUESTIONS'";
        $results = $mysqli->query($query) or die($mysqli->error.__LINE__);
        $total =$results->num_rows;

        $query = "SELECT * FROM 'choices'
                    WHERE question_number = $number AND is_correct=1";
        $results = $mysqli->query($query) or die($mysqli->error.__LINE__);
        $row= $result->fetch_assoc();

        $correct_choice=$row['id'];

        if($correct_choice == $selected_choice){
            $_SESSION['score']++;
        }
        echo $number;
        die();

        if($number == $total){
            header("Location: final.php"); 
            exit();
        } else {
            header("Location: question.php?n=".$next);
        }

    }
?>










//database.php code

<?php
//Create connection credentials
$db_host='localhost';
$db_name='PHPinVisualStudioCode';
$db_user='root';
$db_pass='121212aa';

$mysqli = newmysqli($db_host, $db_user, $db_pass, $db_name);

if($mysqli->connect_error){
    printf("Connect failed: %s\n", $mysqli->connect_error);
    exit();
}
?>


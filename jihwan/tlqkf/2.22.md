# 2/22 내가 배운것
## 이 업계에서 과연 살아갈 수 있을까...

### 자 우선 ```index.php``` 를 생성
```php
<?php
    // echo "" 문자열만 출력
    // print_r(); 배열과 문자열을 출력
    // include_once("") 파일내용 가져오기
    // print_r($_SERVER['REQUEST_FILENAME']);
    include_once("{$_SERVER['DOCUMENT_ROOT']}/page/login.php"); // 내 파일의 경로를 가져오기
?>
```
> 이걸로 localhost:8080/ port 로 접속했을때 기본 경로를 설정해서 하면 편함.
### ```.htaccess``` 라는 설정파일 생성
```
RewriteEngine On 
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(.+)$ index.php?uri=$1 [QSA,L]
```

## POST로 회원가입 기능 구현
```php
<?php
    include_once('./lib/lib.php');
    if(isset($_POST['login'])){
        $read = $pdo->query("select * from member where userId = '{$_POST['userId']}' and userPwd = '{$_POST['userPwd']}'")->fetch();
        print_r($read);
        if($read){
            echo "로그인 성공";
        } else{
            echo "실패";
        }
    }
?>
```
**여기서 중요한게 우선 ```lib/lib.php``` 에 DB 설정을 좀 해줘야함**
```php
<?php 
// pdo 연결하기. 
    $pdo = new PDO("mysql:host=localhost;dbname=HelloWorld;charset=utf8", "root", "", array(
        PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_OBJ,
        PDO::ATTR_ERRMODE => PDO::ERRMODE_WARNING,
    ));

    ...

    // DataBase
    function execute($sql, $parame = null)
    {
        global $pdo;
        $data = $pdo->prepare($sql);
        $data->execute($parame);
        return $data;
    }
    function fetch($sql, $parame = null){
        return execute($sql, $parame)->fetch();
    }
    function fetchAll($sql, $parame = null){
        return execute($sql, $parame)->fetchAll();
    }
    function rowCount($sql, $parame = null){
        return execute($sql, $parame)->rowCount();
    }
?>
```
## 로그인 기능구현
```php
<?php
    include_once('./lib/lib.php');
    if(isset($_POST['login'])){
        $read = $pdo->query("select * from member where userId = '{$_POST['userId']}' and userPwd = '{$_POST['userPwd']}'")->fetch();
        print_r($read);
        if($read){
            echo "로그인 성공";
        } else{
            echo "실패";
        }
    }
?>
```
> 여기서 포인트는 걍 if($read) 했을때 참이면 값이 있는거니 "성공" 없으면 "실패로" ㄲ

### **그리고 요종도 html은 수정해줘야 해**
```html
<form class="login-page" action="" method="post">
    <input type="hidden" name="login" value="">
```
> 이게 뭔 개소리냐  
> action=""을 주고 method를 post 그리고 name을 줘야 됨 그냥 그렇다고.


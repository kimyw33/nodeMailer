# nodeMailer

[Node.js로 간단하게 메일보내기](https://uxicode.tistory.com/entry/Nodejs%EB%A1%9C-%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C-%EB%A9%94%EC%9D%BC%EB%B3%B4%EB%82%B4%EA%B8%B0-nodemailer)  

Nodemailer - [https://nodemailer.com/about/](https://nodemailer.com/about/)  

1. 메일 서버 이용  
- 편의상 구글 계정으로 메일 서버로 고고   
- 보통 계정당 500 명에게 보낼 수 있다 했는데 200명 정도가 max인듯  
- 메일 서버로 사용할 구글 계정을 생성 했다면 아래 주소로 들어가 보안수준이 낮은앱 > '사용함'  으로 설정해야 테스트 메일을 받아볼 수 있다.  
[https://myaccount.google.com/lesssecureapps?pli=1](https://myaccount.google.com/lesssecureapps?pli=1)   

2. 관련 모듈 설치  
```
npm install nodemailer nodemailer-smtp-pool --save
```

3-1. mailer.js(text)
```
var nodemailer = require('nodemailer');



//smtp 서버를 사용하기 위한 모듈이다.

var smtpPool=require('nodemailer-smtp-pool');



//nodemailer 의 createTransport는 transporter 객체를 만드는 메소드인데 

//아래 메소드 참조값 변수 smtpTransport 는 nodemailer-smtp-pool 객체 인스턴스에 인자값으로 쓰인다.

var smtpTransport=nodemailer.createTransport(smtpPool( {
    service:'Gmail',
    host:'localhost',
    port:'465',
    tls:{
        rejectUnauthorize:false
    },

    //이메일 전송을 위해 필요한 인증정보

    //gmail 계정과 암호 
    auth:{
        user:'지메일 계정주소',
        pass:'비밀번호'
    },
    maxConnections:5,
    maxMessages:10
}) );

var mailOpt={
    from:'보내는 사람 주소@gmail.com',
    to:'받는사람주소@gmail.com',
    subject:'Nodemailer 테스트',
    text: '평문 보내기 테스트 '
}
smtpTransport.sendMail(mailOpt, function(err, res) {
    if( err ) {
        console.log(err);
    }else{
        console.log('Message send :'+ res);
    }

    smtpTransport.close();
})
```

3-2. mailer.js(HTML)  
```
var nodemailer = require('nodemailer');

var smtpTransport = nodemailer.createTransport("SMTP", {
	service: 'Gmail',
	auth: {
		user: '구글메일 아이디',
		pass: '구글메일 비밀번호'
	}
});

var mailOptions = {
	from:'보내는 사람 주소@gmail.com',
	to:'받는사람주소@gmail.com',
	subject:'Nodemailer 테스트',
	// text: '평문 보내기 테스트 '
	html:'<h1>HTML 보내기 테스트</h1><p><img src="http://www.nodemailer.com/img/logo.png"/></p>'
};

smtpTransport.sendMail(mailOptions, function(error, response){

	if (error){
		console.log(error);
	} else {
		console.log("Message sent : " + response.message);
	}
	smtpTransport.close();
});
```

3-3. mailer.js(attachment a file)  
```
var nodemailer = require('nodemailer');
var fs = require('fs');

var smtpTransport = nodemailer.createTransport("SMTP", {
	service: 'Gmail',
	auth: {
		user: '구글메일 아이디',
		pass: '구글메일 비밀번호'
	}
});

var mailOptions = {
	from:'보내는 사람 주소@gmail.com',
	to:'받는사람주소@gmail.com',
	subject:'Nodemailer 테스트',
	// text: '평문 보내기 테스트 ',
	html:'<h1>HTML 보내기 테스트</h1><p><img src="http://www.nodemailer.com/img/logo.png"/></p>',
        attachments:
	[
		{
			fileName: 'test.txt',
			path:'./test.txt', // 파일 경로를 써줌
		},
	],
};

smtpTransport.sendMail(mailOptions, function(error, response){

	if (error){
		console.log(error);
	} else {
		console.log("Message sent : " + response.message);
	}
	smtpTransport.close();
});
```

4. 실행  
```
node mailer
```

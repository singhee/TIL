# 파일 업로드

##### 웹에서의 파일 업로드 방법
	1. application/x-www-urlencoded
	2. multipart/form-data

## Multer
	이미지등 바이너리 파일 전송을 위해 Express 에서 제공하는 모듈
	multipart/form-data 방식을 지원해주는 Express 미들웨어

```javascript
/* Create new image */
// 1. 파일명 parameter 를 추가해 업로드 경로 설정 (서버에 저장될 파일이름 받아오기)
// 2. 업로드 결과 파일명과 확장자 리턴
router.post('/:filename', function(req, res, next){
	upload(req, res).then(function(file){
		res.json(file);		// 파일명과 확장자를 포함한 file 객체
	},function(err){
		res.send(500, err);
	});
});

```

- `upload()` 함수 구현
```javascript
var upload = function (req, res){
    var deferred = Q.defer();
	var storage = multer.diskStorage({
		// 서버에 저장할 폴더
		destination: function (req, file, cb){
			cb(null, imagePath);
		},

		// 서버에 저장할 파일 명
		filename: function (req, file, cb){
			file.uploadedFile = {
				name: req.params.filename,
				ext: file.mimetype.split('/')[1]
			};
			cb(null, file.uploadedFile.name + '.' + file.uploadedFile.ext);
		}
	});

	var upload = multer({ storage: storage }).single('file');
	upload(req, res, function(err){
		if(err) deferred.reject();
		else deferred.resolve(req.file.uploadedFile);
	});
	return deferred.promise;
}

```
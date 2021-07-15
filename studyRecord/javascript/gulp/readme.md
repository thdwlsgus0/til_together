### gulp를 이용한 image-sprite 만들기의 여정

#### 1. gulp란?
   - task 기반의 모듈 번들러입니다.
   - 빌드를 자동화해주고, css와 js의 기능을 향상시키기 위한 다양한 모듈들이 존재하기때문에 이를 연동해서 사용하면 다양한 효과를 낼 수 있습니다. (ex. 이미지 스프라이트)


#### 2. gulp task 만들기
   - gulp는 task를 만드는 과정이 정말 간단하다.
   ```javascript
     gulp.task('태스크 이름', '모듈');

     // ex) default라는 값을 출력
     gulp.task('default', function(){
         console.log('default');
     })

    // 실행
    gulp default
   ```

#### 3. gulp를 이용한 이미지스프라이트 제작기
   - 사이드 프로젝트를 진행하다가 저는 이미지를 하나의 이미지 묶음 파일로 만들어서 관리하는게 좋다고 판단하여 이미지 묶음 파일을 만들려고 gulp를 세팅하였습니다.
   
   - 하지만 아래와 같이 주석과 single-quote(ex. 'sprite.png'가 생겼고, 이로 인해서 scss파일이 webpack에서 읽을 수 없는 버그가 발생하였습니다.
   ```css
  
        // SCSS variables are information about icon's compiled state, stored under its original file name
        //
        // .icon-home {
        //   width: $icon-home-width;
        // }
        //
        // The large array-like variables contain all information about a single icon
        // $icon-home: x y offset_x offset_y width height total_width total_height image_path;
        //
        // At the bottom of this section, we provide information about the spritesheet itself
        // $spritesheet: width height image $spritesheet-sprites;
        $alarm-name: 'alarm';
        $alarm-x: 65px;
        $alarm-y: 107px;
        $alarm-offset-x: -65px;
        $alarm-offset-y: -107px;
        $alarm-width: 32px;
        $alarm-height: 32px;
        $alarm-total-width: 598px;
        $alarm-total-height: 161px;
        $alarm-image: 'sprite.png';
        $alarm: (65px, 107px, -65px, -107px, 32px, 32px, 598px, 161px, 'sprite.png', 'alarm', );
        $programmerground-name: 'programmerground';
        $programmerground-x: 0px;

   ```
   - 이러한 문제를 해결하기 위해서 single-quote => double-quote로 바꿔주는 gulp-replace-comments라는 모듈을 사용하였습니다.
   - 또 다른 문제인 주석은 제가 직접 모듈을 만들어서 출력을 만들었습니다.
   - 직접 만든 코드는 다음과 같습니다.
      - 쉽게 이해하면 module.exports를 이용해서 모듈로 분리한 후에 
      - RegExp라는 클래스를 이용해서 모든 주석을 정규식으로 매칭하면서 제거하는 로직입니다.
      - 이 부분에 대해서는 궁금한 부분이 있다면 설명드리겠습니다 ㅎㅎ
   ```javascript
  
        const through2 = require('through2');
        const gutil = require('gulp-util');

        const PLUGIN_NAME = 'gulp-remove-comments'; 

        module.exports = function(){
        return through2.obj(function(file, enc, cb){
            
            const regExp = new RegExp("//.*\n", "gm");

            if(file.isNull()) {
                this.push(file);
                return cb();
            }

            if(file.isStream()){
                this.emit('error', new gutil.PluginError(PLUGIN_NAME, 'Streaming not supported'));
                return cb();
            }

            let contents = file.contents.toString();
            contents = contents.replace(regExp, "").trim();
            
            file.contents = new Buffer(contents);

            this.push(file);
            cb();
        });
        } 
   ```
   
   - gulp를 이용해서 task를 수행합니다.
   - gulp sprite => gulp default 순으로 진행하면 되고, sprite.scss의 주석과 single-quote를 default task에서 제거해줍니다.

   ```javascript
        const gulp = require('gulp');
        const spritesmith = require('gulp.spritesmith');
        const replaceQuote = require('gulp-replace-quotes');
        const removeComments = require('./gulp-remove-comments');

        gulp.task('sprite', function() {
            
            const spriteData = gulp.src('./src/assets/sprite/*.png').pipe(spritesmith({
                imgName: 'sprite.png',
                cssName: 'sprite.scss',
                padding: 5
            }));
            
           return spriteData.pipe(gulp.dest('./src/assets/images'));
        });

        gulp.task('default', function() {
            gulp.src('src/assets/images/sprite.scss')
            .pipe(replaceQuote({
                quote:'double'
            })).pipe(removeComments())
            .pipe(gulp.dest('build'))
        });
   ```

   - gulp를 수행하면 다음처럼 double-quote가 생기고 주석이 제거됩니다.
   
   ```css
        $alarm-name: "alarm";
        $alarm-x: 65px;
        $alarm-y: 107px;
        $alarm-offset-x: -65px;
        $alarm-offset-y: -107px;
        $alarm-width: 32px;
        $alarm-height: 32px;
        $alarm-total-width: 598px;
        $alarm-total-height: 161px;
        $alarm-image: "sprite.png";
        $alarm: (65px, 107px, -65px, -107px, 32px, 32px, 598px, 161px, "sprite.png", "alarm", );
        $programmerground-name: "programmerground";
        $programmerground-x: 0px;
        $programmerground-y: 0px;
        $programmerground-offset-x: 0px;
        $programmerground-offset-y: 0px;
        $programmerground-width: 598px;
        $programmerground-height: 102px;
        $programmerground-total-width: 598px;
        $programmerground-total-height: 161px;
        $programmerground-image: "sprite.png";
   ```


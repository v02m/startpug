# startpug
# Курс верстки ленднинга от ITVDN.com

## Перед стартом проекта необходимо:

* Установить node с сайта nodejs.org (Проверить наличие node можно командой node -v).
* Вместе с node устанавливается npm (Проверить наличие npm можно командой npm -v).
Найти нужные пакеты для npm можно на сайте www.npmjs.com.
* Установить gulp 4 версии:

> npm install gulp-cli -g

В оригинале была следующая команда, но она не сработала:
> npm install gulpjs/gulp-cli#4.0 -g

# Для старта проекта:

* Склонировать репозитарий:
> git clone https://github.com/v02m/itvdn-landing.git

* Запустить команду npm install.
Данная команда установит все пакеты, указанные в файле package.json,
а также все их зависимости.


> echo "# startpug" >> README.md
> git init
> git add README.md
> git commit -m "first commit"
> git remote add origin https://github.com/v02m/startpug.git
> git push -u origin master



##Необходимо убедится что файл gitignore создан и соответствует проекту
### После этого создаем файл package.json 
> npm init

#Устанавливаем локально нужные пакеты а именно


> npm install gulpjs/gulp#4.0 --save-dev

>  npm install browser-sync --save-dev

>  npm install gulp-pug gulp sass --save-dev

##Создаем файл gulpfile.js

const gulp = require('gulp');
const pug = require('gulp-pug');
const sass = require('gulp-sass');
const spritesmith = require('gulp.spritesmith');
const browserSync = require('browser-sync').create();
const rimraf = require('rimraf');
const rename = require('gulp-rename');


// Compilling Pug to html
gulp.task('templates:compile', function buildHTML() {
  return gulp.src('src/templates/index.pug')
  .pipe(pug({
    pretty: true
  }))
  .pipe(gulp.dest('build'))
});


// Style compile
gulp.task('styles:compile', function () {
  return gulp.src('src/styles/main.scss')
    .pipe(sass({outputStyle: 'compressed'}).on('error', sass.logError))
    .pipe(rename('main.min.css'))
	.pipe(gulp.dest('build/css'));
});

// Sprites
gulp.task('sprite', function () {
  const spriteData = gulp.src('src/images/icons/*.png').pipe(spritesmith({
    imgName: 'sprite.png',
    cssName: 'sprite.css'
  }));
  return spriteData.pipe(gulp.dest('build/images/'));
  return spriteData.pipe(gulp.dest('src/styles/global'))
  cb();
});

// Delete
gulp.task('clean', function del(cb){
	return rimraf('build', cb);
});

// Static server
gulp.task('brs', function() {
    browserSync.init({
        server: {
			port: 9000,			
            baseDir: 'build'
        }
    });
	
	gulp.watch('build/**/*.*').on('change', browserSync.reload);
	
	gulp.watch('build/*.html').on('change', browserSync.reload);
	
});



/*---------- Copy fonts-----------*/
gulp.task('copy:fonts', function(){
	return gulp.src('/src/fonts/**/*.*')
	.pipe(gulp.dest('build/images'));
});

/*---------- Copy fonts-----------*/
gulp.task('copy:images', function(){
	return gulp.src('/src/images/**/*.*')
	.pipe(gulp.dest('build/images'));
});

/*---------- Copy fonts-----------*/
gulp.task('copy', gulp.parallel('copy:fonts', 'copy:images'));

/*--- Watchers----*/
gulp.task('watch', function(){
	gulp.watch('src/templates/**/*.pug', gulp.series('templates:compile'));
	gulp.watch('src/styles/**/*.scss', gulp.series('styles:compile'));
});

/*-- Default --*/
gulp.task('default', gulp.series(
	'clean',
	gulp.parallel('templates:compile', 'styles:compile', 'sprite', 'copy'),
	gulp.parallel('watch', 'brs')
    )
);



#Полезные сайты
https://www.npmjs.com/
https://www.npmjs.com/package/gulp

https://github.com/gulpjs/gulp/
https://www.browsersync.io/docs/gulp

/* gulp.watch('build/**/*.scss').on('change', browserSync.reload); */
# A commandline driven blog created with the aid of Gulp, Jade, Markdown, and SASS

```js
var gulp         = require('gulp');
var sass         = require('gulp-sass');
var sourcemaps   = require('gulp-sourcemaps');
var autoprefixer = require('gulp-autoprefixer');
var csslint      = require('gulp-csslint');
var jshint       = require('gulp-jshint');
var jscs         = require('gulp-jscs');
var stylish      = require('jshint-stylish');
var jade         = require('gulp-jade');
var rename       = require('gulp-rename');
var browserSync  = require('browser-sync').create();
var reload       = browserSync.reload;

gulp.task('sass', function() {
  gulp.src('assets/sass/style.sass')
    .pipe(sourcemaps.init())
    .pipe(sass({
      indentedSyntax: true,
      outputStyle: 'compressed',
      errLogToConsole: true
    }))
    .pipe(autoprefixer())
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('.'))
    .pipe(reload({stream: true}));
});

gulp.task('csslint', function() {
  gulp.src('style.css')
    .pipe(csslint('.csslintrc'))
    .pipe(csslint.reporter());
});

gulp.task('jshint', function() {
  gulp.src(['gulpfile.js'])
    .pipe(jshint('.jshintrc'))
    .pipe(jshint.reporter(stylish));
});

gulp.task('jscs', function() {
  return gulp.src(['gulpfile.js'])
    .pipe(jscs());
});

gulp.task('indexJade', function() {
  gulp.src('.templates/index.jade')
    .pipe(jade({
      pretty: true
    }))
    .pipe(gulp.dest('.'))
    .pipe(reload({stream: true}));

});

gulp.task('postsJade', function() {
  gulp.src('posts/**/*.jade')
    .pipe(jade({
      pretty: true
    }))
    .pipe(reload({stream: true}));

});

gulp.task('resumeJade', function() {
  gulp.src('resume/*.jade')
    .pipe(jade({
      pretty: true
    }))
    .pipe(reload({stream: true}));

});

gulp.task('browser-sync', function() {
  browserSync.init({
    server: true,
    notify: false,
    ghostMode: {
      clicks: true,
      forms: true,
      scroll: true
    },
    logConnections: true,
    open: true,
    port: 8001
  });
});

gulp.task('watch', function() {
  gulp.watch('assets/sass/**/*', ['sass']);
  gulp.watch('style.css', ['csslint']);
  gulp.watch('templates/**/*.jade', ['indexJade', 'postsJade']);
  gulp.watch(['gulpfile.js'], ['jscs', 'jshint']);
  gulp.watch('**/*.md', ['indexJade', 'postsJade']);
});

// Default Task
gulp.task('default', [
  'sass',
  'csslint',
  'jshint',
  'jscs',
  'indexJade',
  'postsJade',
  'browser-sync',
  'watch'
]);
```

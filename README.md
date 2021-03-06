# Super gulp

![Main](assets/main-screen.png)

π [Deploy Site](https://jinyongp.dev/super-gulp/)

Learn Gulp by building an awesome development environment

## Scripts

- `yarn`: Install dependencies
- `yarn dev`: Start live server for development
- `yarn build`: Build this project
- `yarn deploy`: Deploy this project

## Learning Gulp

### Scaffolding

[Gulp](https://gulpjs.com/)λ₯Ό νμ΅νκΈ° μν΄ μΌλ°μ μΈ κ°λ° νκ²½μ κ΅¬μ±νμ΅λλ€. Pug, SCSS κ·Έλ¦¬κ³  μ΅μ  JS λ¬Έλ²μ μ¬μ©νλ νκ²½μμ λΈλΌμ°μ μμ κ΅¬λλ  μ μλλ‘ gulpλ‘ λΉλνλ κ² λͺ©μ μλλ€.

```
.
βββ .gitignore
βββ README.md
βββ package.json
βββ yarn.lock
βββ src
    βββ index.pug
    βββ img
    β   βββ logo.png
    βββ js
    β   βββ main.js
    β   βββ util.js
    βββ partials
    β   βββ footer.pug
    β   βββ header.pug
    βββ scss
    β   βββ _reset.scss
    β   βββ _variables.scss
    β   βββ style.scss
    βββ templates
        βββ layout.pug
```

### Configuring Babel for Gulp

`gulp` λͺ¨λμ μ€μΉνκ³  `gulp` λͺλ Ήμ΄λ₯Ό μ€ννκΈ° μν΄μ `package.json`μ `scripts`λ₯Ό μΆκ°ν΄μ£Όμμ΅λλ€.

```json
{
  "scripts": {
    "dev": "gulp dev",
    "build": "gulp build"
  },
}
```

`gulp` λͺλ Ήμ΄λ₯Ό μ€ννκΈ° μν΄μ  μΆκ°λ‘ [`gulpfile.js` νμΌ](https://gulpjs.com/docs/en/getting-started/javascript-and-gulpfiles)μ΄ νμν©λλ€. `gulpfile.js`μμ `import/export` λ¬Έλ²μ μ¬μ©νκΈ° μν΄ [babel](https://babeljs.io/)μ μ€μΉν©λλ€.

```sh
$ yarn add -D @babel/core @babel/register @babel/preset-env
```

κ·Έλ¦¬κ³  [`gulpfile.babel.js`](gulpfile.babel.js)λ‘ μ΄λ¦μ λ³κ²½ν©λλ€. μΆκ°λ‘ [`.babelrc`](.babelrc)μ μμ±ν©λλ€. μ΄λ‘μ¨, `gulpfile`μμ μ΅μ  JS λ¬Έλ²μ μμ±ν  μ μκ² λμμ΅λλ€.

## Pug Compilation

`scripts`μμ μμ±ν λͺλ Ήμ΄λ₯Ό μ€ννλ©΄ `src` ν΄λ λ΄μ νμν νμΌμ μ»΄νμΌν΄μΌ ν©λλ€. `dev` λͺλ Ήμ΄λ₯Ό μ€ννκ³  μΆλ€λ©΄ `gulp.babel.js`μμ `dev`λ₯Ό `export` ν΄μΌν©λλ€.

```js
import gulp from 'gulp';

export const dev = console.log('scripts:dev');
```

κ·ΈλΌ λ¨Όμ  pug νμΌμ μ»΄νμΌνκΈ° μν΄ [`gulp-pug`](https://www.npmjs.com/package/gulp-pug)λ₯Ό μ€μΉνκ³  μλ λ΄μ©μ μμ±ν©λλ€. μ΅μμ pug νμΌ νλλ§ λΉλν©λλ€.

```js
import pug from 'gulp-pug';

const html = () => gulp.src('src/index.pug').pipe(pug()).pipe(gulp.dest('dist'));

export const dev = gulp.series(html);
```

[`src()`](https://gulpjs.com/docs/en/api/src)λ νμΌ μμ€νμΌλ‘λΆν° [`globs`](https://gulpjs.com/docs/en/getting-started/explaining-globs/)λ‘ μ νν νμΌμ μ½μ΅λλ€. μ νν νμΌμ `pipe()` λ©μλλ₯Ό μ°κ²°νμ¬ λ³νν©λλ€. `pug` νμΌμ μ»΄νμΌνκΈ° μν΄ `gulp-pug`λ₯Ό μ°κ²°ν©λλ€. μ»΄νμΌν κ²°κ³Όλ₯Ό [`dest()`](https://gulpjs.com/docs/en/api/dest)λ‘ μνλ ν΄λμ μ μ₯ν©λλ€.

λ€μ λΉλκ° νμν  λ `dist` ν΄λλ₯Ό μ­μ νλ €λ©΄ [`del`](https://www.npmjs.com/package/del) λͺ¨λμ μ€μΉνκ³  μλ λ΄μ©μ μΆκ°ν©λλ€.

```js
import del from 'del';

const clean = () => del('dist');

export const dev = gulp.series(
  gulp.parallel(clean),
  gulp.parallel(html),
);
```

μ­ν μ λ°λΌ λ¨κ³λ₯Ό λλκΈ° μν΄ [`parallel()`](https://gulpjs.com/docs/en/api/parallel) λ©μλλ‘ μμλ₯Ό κ΅¬λΆνμ΅λλ€.

## Webserver and Watching Files

λΉλ κ²°κ³Όλ¬Όμ μ€ννκΈ° μν΄ [`gulp-webserver`](https://www.npmjs.com/package/gulp-webserver)λ₯Ό μ€μΉνκ³  `gulpfile`μ μΆκ°ν©λλ€.

```js
import webserver from 'gulp-webserver';

const ws = () => gulp.src('dist').pipe(webserver({ livereload: true }));

export const dev = gulp.series(
  gulp.parallel(clean),
  gulp.parallel(html),
  gulp.parallel(ws),
);
```

`dev` λͺλ Ήμ΄λ₯Ό ν΅ν΄ μλ²λ₯Ό μ€νν  μ μμ΅λλ€.

νμΌ λ΄μ©μ λ³κ²½νμ λ μμμ λΉλνκ³  μλ²λ₯Ό μ¬μμν  μ μλλ‘ [`watch()`](https://gulpjs.com/docs/en/api/watch) λ©μλλ₯Ό μ΄μ©ν©λλ€.

```js
const watch = () => gulp.watch('src/**/*.pug', html);

export const dev = gulp.series(
  gulp.parallel(clean),
  gulp.parallel(html),
  gulp.parallel(ws, watch),
);
```

`src` ν΄λ λ΄ λͺ¨λ  `pug` νμΌμ λν΄ λ³κ²½μ¬ν­μ΄ λ°μνλ€λ©΄ `pug`λ₯Ό λ€μ μ€ννμ¬ λΉλν©λλ€.

## Image Optimization

[`gulp-image`](https://www.npmjs.com/package/gulp-image) λͺ¨λλ‘ μ΅μ νν μ΄λ―Έμ§λ₯Ό λΉλμ μ λ¬ν  μ μμ΅λλ€.

```js
import image from 'gulp-image';

const img = () => gulp.src('src/img/*').pipe(image()).pipe(gulp.dest('dist/img'));

export const dev = gulp.series(
  gulp.parallel(clean),
  gulp.parallel(html, img),
  gulp.parallel(ws, watch),
);
```

## SCSS Compilation

SCSSλ₯Ό λΉλνκΈ° μν΄ [`gulp-sass`](https://www.npmjs.com/package/gulp-sass)(μΆκ°λ‘ `sass`λ)λ₯Ό μ€μΉνκ³  μλ λ΄μ©μ μΆκ°ν©λλ€.

```js
import dartSass from 'sass';
import gulpSass from 'gulp-sass';

const sass = gulpSass(dartSass);

const css = () =>
  gulp
    .src('src/scss/style.scss')
    .pipe(sass().on('error', sass.logError))
    .pipe(gulp.dest('dist/css'));

const watch = () => {
  gulp.watch('src/**/*.scss', css);
};

export const dev = gulp.series(
  gulp.parallel(clean),
  gulp.parallel(html, css, img),
  gulp.parallel(ws, watch),
);
```

SCSS νμΌμ λ³κ²½μ¬ν­λ μΆμ νκΈ° μν΄ `watch`μ μΆκ°ν΄λ‘λλ€.

CSSμ λν΄ λΈλΌμ°μ  νΈνμ±μ λμ΄κΈ° μν΄ [`gulp-autoprefixer`](https://www.npmjs.com/package/gulp-autoprefixer)λ₯Ό μΆκ°ν©λλ€. λμμ CSS νμΌ μμΆμ μν΄ [`gulp-csso`](https://www.npmjs.com/package/gulp-csso)λ₯Ό μ€μΉνκ³  μλ λ΄μ©μ μΆκ°ν©λλ€.

```js
import autoprefixer from 'gulp-autoprefixer';
import csso from 'gulp-csso';

const css = () =>
  gulp
    .src('src/scss/style.scss')
    .pipe(sass().on('error', sass.logError))
    .pipe(autoprefixer())
    .pipe(csso())
    .pipe(gulp.dest('dist/css'));
```

autoprefixerλ [`browserslist`](https://github.com/browserslist/browserslist)λ₯Ό μ§μν©λλ€. [μ΄ μ€ νλλ₯Ό μ ν](https://github.com/browserslist/browserslist#full-list)νκ³  [`.browserslistrc`](.browserslistrc) νμΌμ μμ±νμ¬ μ§μ λ²μλ₯Ό μ νν  μ μμ΅λλ€.

## Babel Support

`import/export` λ¬Έλ²κ³Ό λλΆμ΄ μ΅μ  JS λ¬Έλ²μ λν΄ λΈλΌμ°μ  νΈνμ±μ λμ΄κΈ° μν΄μ [`gulp-bro`](https://www.npmjs.com/package/gulp-bro), [`babelify`](https://www.npmjs.com/package/babelify), [`uglifyify`](https://www.npmjs.com/package/uglifyify)λ₯Ό μ€μΉνκ³  λ€μ λ΄μ©μ μΆκ°ν©λλ€.

```js
import browserify from 'gulp-bro';
import babelify from 'babelify';

const js = () =>
  gulp
    .src('src/js/main.js')
    .pipe(
      browserify({
        debug: process.env.NODE_ENV === 'development',
        transform: [
          babelify.configure({ presets: ['@babel/preset-env'] }),
          ['uglifyify', { global: true }],
        ],
      }),
    )
    .pipe(gulp.dest('dist/js'));

const watch = () => {
  gulp.watch('src/**/*.js', js);
};

export const dev = gulp.series(
  gulp.parallel(clean),
  gulp.parallel(html, css, js, img),
  gulp.parallel(ws, watch),
);
```

- [`browserify`](https://browserify.org/)λ λΈλΌμ°μ κ° `require` λ¬Έλ²μ μ΄ν΄ν  μ μλλ‘ ν©λλ€.
- [`babelify`](https://github.com/babel/babelify)λ `browserify`μ© transformμΌλ‘ μ΅μ  JS λ¬Έλ²μ΄ λλΆλΆμ λΈλΌμ°μ μ μ€ν κ°λ₯νλλ‘ νΈλμ€νμΌλ§ν©λλ€.
- [`uglifyify`](https://www.npmjs.com/package/uglifyify)λ μ½λλ₯Ό μμΆνμ¬ μ©λμ μ€μλλ€.

κ°λ° λͺ¨λμΌ κ²½μ°, `debug` μ΅μμ μΆκ°νμ¬ SourceMap νμΌμ΄ μμ±λλλ‘ νμ¬ κ°λ° νκ²½μμ λλ²κ·Έλ₯Ό μ©μ΄νκ² ν©λλ€.

## Deploying

[`gulp-gh-pages`](https://www.npmjs.com/package/gulp-gh-pages)λ‘ νλ‘μ νΈλ₯Ό λ°°ν¬ν©λλ€.

```js
import pages from 'gulp-gh-pages';

const clean = () => del(['dist', '.publish']);

const publish = () => gulp.src('dist/**/*').pipe(pages());

const prepare = gulp.parallel(clean);
const assets = gulp.parallel(html, css, js, img);
const live = gulp.parallel(ws, watch);

export const build = gulp.series(prepare, assets);
export const dev = gulp.series(build, live);
export const deploy = gulp.series(build, publish, clean);
```

[`package.json`](package.json)μ `scripts:deploy`λ₯Ό μΆκ°ν©λλ€. `gh-pages`λ‘ λ°°ν¬νκ³  μμ±λ `dist` ν΄λμ `.publish` ν΄λλ₯Ό μ λ¦¬ν©λλ€.

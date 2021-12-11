# Movie Search App with Vue 3
## Vue 3 ile Film Arama Uygulaması

Yıllar önce HTML5, SCSS/CSS3 ve jQuery teknolojileri ile geliştirdiğim ve filmleri OMDb API'sini sorgulayarak listelediğim bir uygulamaydı. Şimdi **Vue 3** ile biraz daha geliştirerek yeniden kodladım.

### Kullanım
OMDb Api key oluşturmak için https://www.omdbapi.com/apikey.aspx linkine gidin.
Generate API Key bölümünden API anahtarınızı oluşturup **src/App.vue** dosyasında **apiKey** attribute'una ekleyin.

### English
It was an application that I developed years ago with HTML5, SCSS/CSS3 and jQuery technologies and I listed movies by querying the OMDb API. Now I've re-coded it with **Vue 3** by developing it a bit more.

### Usage
To generate OMDb Api key, go to https://www.omdbapi.com/apikey.aspx.
Generate your API key from the Generate API Key section and add it to the **apiKey** attribute in the **src/App.vue** file.

<br>

```html
<Movies apiKey="API_KEY" ... />
```

<br>

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).


<br><br><img alt="Movie Search App with Vue 3" src="src/assets/images/screenshot_1.jpg">

<br><img alt="Movie Search App with Vue 3" src="src/assets/images/screenshot_2.jpg">
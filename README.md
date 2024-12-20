# Neovim like theme 

Neovim theme is a neovim like theme for zola.

![screenshot](./screenshot.png)

exemple: [https://super-botman.github.io](https://super-botman.github.io)

## Instalation
```bash
cd themes
git clone https://github.com/Super-Botman/neovim-theme.git
```

then enable it in your config

```toml
theme = "neovim-theme"
```

## Config

You can setup the blog name with config file in extra

```toml
[extra]
blog_name = "name"
```

## Customisation

### JS

You can add some custom javascript function with this parameter:

```toml
[extra]
custom_script = "<path>.js" 
```
then you just add a file `static/js/custom_script.js` and define your custom functions like this:

```javascript
// add special commands
function custom_commands(command, args){
   ...
}

// add special init routine
function custom_init(){
    ...
}
```

### CSS

And for css 

```toml
[extra]
custom_css = "<path>.css"
```

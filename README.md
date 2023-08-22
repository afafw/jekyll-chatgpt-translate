<img src="logo.png" style="width:256px;"/>

[![rake](https://github.com/yegor256/jekyll-chatgpt-translate/actions/workflows/rake.yml/badge.svg)](https://github.com/yegor256/jekyll-chatgpt-translate/actions/workflows/rake.yml)
[![Gem Version](https://badge.fury.io/rb/jekyll-chatgpt-translate.svg)](http://badge.fury.io/rb/jekyll-chatgpt-translate)

If you have a [Jekyll](https://jekyllrb.com/) static site, this plugin may help you automatically
translate its pages to another language, through [ChatGPT](https://chat.openai.com/). See how it 
works for [my blog](https://github.com/yegor256/ru.yegor256.com), 
for example [this page](https://ru.yegor256.com/2023-08-13-dictators.html) is translated to 
[English](https://ru.yegor256.com/english/2023-08-13-dictators.html).

Install it first:

```
gem install jekyll-chatgpt-translate
```

Then, add this to `_config.yml`:

```yaml
plugins:
  - ... your other plugins here ...
  - jekyll-chatgpt-translate
chatgpt-translate:
  model: gpt-3.5-turbo
  source: en
  layout: translated
  targets: 
    - 
      language: zh
      permalink: :year-:month-:day-:slug-chinese.html
      layout: chinese-translated
    - 
      language: fr
      permalink: :year-:month-:day-:title-french.html
```

Here, the source language is English (`en`), the target one is Chinese (`zh`),
the layout is `_layout/translated.html` (you must have this file).

OpenAI API KEY must be set in `OPENAI_API_KEY` environment variable, otherwise
the plugin will not do any translation and won't generate translated pages. 
You can get your key [here](https://help.openai.com/en/articles/4936850-where-do-i-find-my-secret-api-key).

Inside the original page you can use `{{ page.translated-XX-url }}` in order to render the URL
of the translated page, where `XX` is the [ISO-639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 
code of the target language.
Inside the translated page you can use `{{ page.translated-original-url }}` in order
to get the URL of the page that was translated. 

You can also use `{{ page.chatgpt-model }}`
inside both the original page and the translated one, to refer to the model of ChatGPT.
The presence of this attribute in the `{{ page }}` means that the
page was translated or the translated HTML was downloaded and placed into the `_site` directory.

## Options

Full list of options available to specify in `_config.yml`:

  * `api_key_file` (optional) — the file with OpenAI API key. If this option is not specified,
    it is expected to have the key in the `OPENAI_API_KEY` environment variable.

  * `model` (optional) — specifies the model to use by ChatGPT.

  * `source` (optional) — is the ISO-839-1 code of the source language.

  * `no_download` (optional) — if this attribute is present, the plugin won't try
    to find HTML versions of translated pages in the Internet and won't try to 
    download them and place into the `_site` directory. Thus, your entire site
    will have to be re-translated on every build (might be very ineffective if the site is big!)

  * `layout` (optional) — is name of the file in `_layouts` directory, without the extension. 
This layout will be specified for the pages generated by this plugin.

  * `targets` (mandatory) — an array of target languages, each of which has the following attributes

    * `language` (mandatory) — ISO-839-1 code of the target language

    * `permalink` (mandatory) — template to use for newly generated pages

    * `layout` (optional) — the name of the file in the `_layouts` directory

  * `threshold` (optional) — maximum number of pages to generate in one build cycle.
    The default value is 1024. It is recommended to use smaller number, in order
    to avoid too long builds. You can re-run the build again and missing pages
    will be generated. Thus, in a few builds the entire site will be translated.

  * `version` (optional) — the version that will be attached to each generated page, 
    in order to avoid repetitive translations on one hand and enable re-translations
    when the `version` is changed on another hand. By default, the version of
    the plugin will be used here, unless you set your own value.

## How to Contribute

Make a fork and then test it locally like this:

```bash
$ bundle update
$ bundle exec rake
```

If it works, make changes, test again, and then submit a pull request.

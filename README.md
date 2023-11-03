# How to manage & synchronize IMPLE/site templates, menu and snippets across multiple pages?
By creating a GitHub repository and connecting it to your IMPLE/site or sites, you can manage unlimited templates, menu and snippets (assets) on unlimited sites from one place. 

All assets created and synced with GitHub repository get github_ prefix and can't be modified via IMPLE/site admin.

You can add new assets inside GitHub repository, but if you delete any, all synced IMPLE/sites will keep last version of the asset so there is no possibility to break the site.

You can still create individual copies of every such asset via you site Admin panel and use them instead.

There is no better way to deploy and manage multiple sites usin simmilar or the same desing.

You can create:

## templates

Just create template folder inside main tree of your repository and follow this structure:


    templates/
      ├─ template_name/
        ├─ .conf
        ├─ head.html
        ├─ style.css
        ├─ body.html

Other files in a folder will be ignored. You can for example put here README.md file for yourself. Do not put big files inside a repository as unreasonably big repositories can be ignored by us.
 
### templatename folder

`template_name` has to be lowercase alphanumeric string with an optional underscore character/s. It will appear in your sites's Admin panel as `github_template_name`. Do not add `github ` prefix to the template name inside your repository - template will be ignored.

### .conf file

`.conf` is a configuration file that for now accepts one variable:

`htmleditor=ckeditor|ace|implepage`

`htmleditor` variable defines an editor used while editing pages with this template. It curently accepts **one** of this values

- `ckeditor` is a Word like rich-text editor: [https://ckeditor.com](https://ckeditor.com)
- `ace` is a rich code editor: [https://ace.c9.io](https://ace.c9.io)
- `implepage` is a visual editor based on ready made sections that can be tested on [https://imple.page](https://imple.page). This editor automatically loads IMPLE/page css and javascript into a template.

`.conf` file has a structure of:

- one variable = value pair per line
- variable followed by and equal sign and followed by value
  
Variables are always alphanumeric strings with an optional underscore character/s. Case of a variable name does not matter.

White spaces before or after a variable or value does not matter.

You can add comments prefixed with hash sign # in separate lines or AFTER a value. Everything after hash sign in a line will be ignored. Adding hash before a variable name will disable it.

Empty lines will be ignored.

### head.html file

`head.html` file contains every html declaration that you want to put inside `<head>` and `</head>` tags of a page, like javascript, css or individual meta tags.

Do not put here TITLE or META description as they are defined for every page via Admin panel. 

DOCTYPE declaration is defined in snippets also in an Admin panel.

There are a lot of available dynamic flags that can be used here that you will find in a IMPLE/site admin panel while editing any template.

You can put here snippets `{HTML:snippet_name}` defined via this repository (with `github_` pefix) or defined individually via site's Admin panel. 

You can put here setup variables `{SETUP:setup_name}` defined via via an Admin panel.

Is you put here snippets or setup variables that are not defined for a particular site, nothing will be rendered on a live page, but there will be a notice in an Admin panel so page administrator can define it and make it individual.

If you want to make every site has it’s own look, you can put here some style declarations like that:

Let every site declare individual styles

    <style>
      {HTML:site_styles}
    </style>

Let every site declare individual part of a HEAD section. Preferably it shoudl apper at the bottom of `head.html` file.

    {HTML:site_custom_head}

Declare standard elements and their parameteres individually via setup variables of every site. ATTENTION: Can break site look if particular setup variable is not set.

    <style>
    element {
    background-color:{SETUP:main_bgcolor};
    }
    </style>

Declare standard css file individually uploaded to every site's files repository. ATTENTION: Can show errors or slow down the site if file will not be uploaded.

    <link rel="stylesheet" href="{PAGE:host}/custom/site_styles.css" TYPE="text/css">
    <script src="{PAGE:host}/custom/site_scripts.js" type="text/javascript"></script>

### style.css file

You can declare template individual styles here. Anything that would work in a typical css file can go here.

Snippets and setup variables DO NOT work here. Check `head.html `file instructions for individualisation examples.

### body.html file

`body.html` file contains every html declaration that you want to put inside `<body>` and `</body>` tags of a page. It’s a main structure of a page.

`body.html` should contain `{CONTENT:}` flag is you want to display content defined for a particular site.

There are a lot of available dynamic flags that can be used here that you will find in a IMPLE/site admin panel while editing any template.

You can put here menu `{MENU:menuname}` defined via this repository (with `github_` pefix) or defined via an Admin panel.  

You can put here snippets `{HTML:snippetname}` defined via this repository (with `github_` pefix) or defined via an Admin panel. 

You can put here setup variables `{SETUP:setupname}` defined via via an Admin panel.

Is you put here snippets or setup variables that are not defined for a particular site, nothing will be rendered on a live page, but there will be a notice in an Admin panel so page administrator can define it and make it individual.


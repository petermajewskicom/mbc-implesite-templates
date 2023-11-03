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
      ├─ other_template_name/

Other files in a folder will be ignored. You can for example put here README.md file for yourself. Do not put big files inside a repository as unreasonably big repositories can be ignored by us.
 
### template_name folder

`template_name` has to be lowercase alphanumeric string with an optional underscore character/s not longer than 30 characters. It will appear in your sites's Admin panel as `github_template_name`. Do not add `github ` prefix to the template name inside your repository - template will be ignored.

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

## menu

Just create menu folder inside main tree of your repository and follow this structure:


    menu/
      ├─ menu_name/
        ├─ .conf
        ├─ level1.html
        ├─ level2.html
        ├─ level3.html
        ├─ level4.html
        ├─ level5.html
      ├─ other_menu_name/

Other files in a folder will be ignored. You can for example put here README.md file for yourself. Do not put big files inside a repository as unreasonably big repositories can be ignored by us.
 
### menu_name folder

`menu_name` has to be lowercase alphanumeric string with an optional underscore character/s not longer than 30 characters. It will appear in your sites's Admin panel as `github_menu_name`. Do not add `github ` prefix to the menu name inside your repository - menu will be ignored.

### .conf file

`.conf` is a configuration file that by default has this variables and values:

    parent = -1      # which page submenu to show, -99 all pages without any structure, -2 current page parent, -1 current page, 0 main pages, 1+ - id of a page
    levels = 1       # how many levels generate
    level = 1        # from which level of subpages start
    auth = 0         # minimal auth level, 0 - all or 1-10
    print = 0        # unactive
    sort = p.aid     # p.aid = page id, p.name = page name, p.published = page published date, p.modified = page modified date, p.auth = page minimal auth level, p.creator = page creator id
    order = asc      # ordering asc or desc
    limit = 0        # 0 no limit or a number, prefix with GET to modify by &limit=X parameter in a url, eg. GET20 will display 20 pages unless there is a ?limit= parametr changing it 
    start = 0        # 0+, where to start, prefix with GET to modify by &start=X parameter in a url, eg. GET0 will display pages from the first one unless there is a &start= parametr changing it

If you do not declare any of this variables or declare them incorrectly, default variable will be used.

`.conf` file has a structure of:

- one variable = value pair per line
- variable followed by and equal sign and followed by value
  
Variables are always alphanumeric strings with an optional underscore character/s. Case of a variable name does not matter.

White spaces before or after a variable or value does not matter.

You can add comments prefixed with hash sign # in separate lines or AFTER a value. Everything after hash sign in a line will be ignored. Adding hash before a variable name will disable it.

Empty lines will be ignored.

### levelX.html file

`levelX.html`, where X is a number from 1 do 5, defines how every level of menu will be displayed.

To uderstand it better is we have that kind of menu:

    <ul>
        <li><a href="/">Start</a></li>
        <li><a href="/services">Services</a></li>
        <li><a href="/contact">Contact</a></li>
    </ul>

We can create new menu `menu_name` with `level1.html` file:

    <li><a href="{URL}">{PAGE:name}</a></li>

And `.conf` file:

    parent = 0     # display root level pages
    levels = 1     # only first level
    order = asc    # ascending sorting   
    sort = p.aid   # by page id number

Or if we want a granular control on a sorting, we can add `sort = p.published` and set publishing dates of every page to achieve desired order. Date of publishing has to be set manually. Modified date is set dynamically.

And then insert it dynamically into template or page content like that:

    <ul>
        {MENU:menu_name}
    </ul>

or to show subpages of current page:

    <ul>
        {MENU:menu_name:{PAGE:aid}}
    </ul>

or to show subpages of a page with id for example 5:

    <ul>
        {MENU:menu_name:5}
    </ul>

Menu system in IMPLE/site is crazy elastic. You can create very complicated structures, blog lists with pages etc. Check edit page of any menu in an Admin panel to learn more.


## snippets

Just create snippets folder inside main tree of your repository and follow this structure:


    snippets/
      ├─ snippet_name.(js|html|css)
      ├─ other_snippet_name.(js|html|css)

Files with extensions other than `.js`, `.html` or `.css` will be ignored. You can for example put here README.md file for yourself. Do not put big files inside a repository as unreasonably big repositories can be ignored by us.
 
### snippet_name.(js|html|css) file

`snippet_name.(js|html|css)` has to be lowercase alphanumeric string with an optional underscore character/s not longer than 30 characters (including extension). 

The extension is only for you to easily understand what is inside the snippet. For example a file `snippet_name.js` will be added to site's snippets repository as `github_snippet_name_js` with tags: `github` and `js`

Inside a file should be snippet code that templates, other snippets, page snippets od page content can use. It can be html, css, javascript or mix of this 'languges'.

Basically anything that is repeated through multiple pages/templates should be put into a snippet. So is you create multiple templates with simillar elements, put those elements into a snippet.

Snippets can have other snippets, setup variables or menu tags inside. 

You can also create simple functions with snippets. Put `{VARX}` tags into a snippet (where X is a number 1 to 9) where you want function parameters appear. Those tags are then declared this way: `{HTM:snippet_name:Variable 1 content|Variable 2 content|...}` - so it works like a simple function.

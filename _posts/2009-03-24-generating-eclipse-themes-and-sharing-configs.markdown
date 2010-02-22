---
layout: post
title: Generating Eclipse Themes and Sharing Configs
disqus_url: http://timrobles.com/blog/entry/generating_eclipse_themes_and_sharing_environments/
---

Here's a quick post on how to generate themes for Eclipse that you can share across your development machines (home and work for me). It goes back to the age old dilemma of having a consistent IDE across platforms. Purists might use VIM since it's everywhere, or Eclipse since it's still decently cross platform. In any case you'd want to save common settings that you like in a .vimrc file, templates XML or color schemes (all your dot files). You can see my configs on [Github](http://github.com/roblocop/trdotcom/tree/master/lib/prefs). Creating an Eclipse them is really just exporting a preference file that only has color specific information in it.
1. Export your Eclipse preferences using File > Export > General > Preferences. This will output a .epf file with a whole bunch of preferences including repository info; way more than you need.
2. Run a simple "cat my_prefs.epf | grep -i color > my_color_prefs.epf" command to create a preference file that contains color information only.
3. Add "file_export_version=3.0" as the top line of your new epf file. This allows the file to imported into Eclipse >= 3.0. Without this your preferences will not be imported.

That's it! You should be on your way to happy and comfortable coding where ever you are. Be sure to check out [Joe Ferris' Github](http://github.com/jferris/config_files/tree/master) for more examples of dot file configs. Thanks to [WJ Warren](http://blog.ansuz.nl/index.php/2009/01/26/howto-create-a-color-scheme-for-fdt/) and [Stack Overflow](http://stackoverflow.com/questions/481035/is-there-a-simple-consistent-way-to-change-the-color-scheme-of-eclipse-editors) for pointing me in the right direction.
---
title: Creating custom field types in SuiteCRM
date: 2015-10-30T12:00:00+01:00
author: Jim Mackin
tags: []
source: http://www.jsmackin.co.uk/suitecrm/creating-custom-field-type-suitecrm/
hidden: true
---

Like a lot of SuiteCRM the field types are customisable and you can add
your own types of fields. This post will explain how to add a colour
picker as a custom field type.

<!--more-->

First off we need to add the option to studio to allow creating fields
of our new type. We do this by adding a new file
`custom/modules/DynamicFields/templates/Fields/TemplateColourPicker.php`
with the following contents:

<<<<<<< HEAD:content/community/future-blog-posts/5.creating-custom-field-types.adoc
.Example
[source,php]
=======
```php
>>>>>>> 16d81cf7aa486cd983f69bd15ee5acf2eb4c8942:content/blog/creating-custom-field-types.md
<?php
if(!defined('sugarEntry') || !sugarEntry) die('Not A Valid Entry Point');
                            //
require_once('modules/DynamicFields/templates/Fields/TemplateField.php');
class TemplateColourPicker extends TemplateField{
    var $type='ColourPicker';
<<<<<<< HEAD:content/community/future-blog-posts/5.creating-custom-field-types.adoc
                            //    
=======
.
>>>>>>> 16d81cf7aa486cd983f69bd15ee5acf2eb4c8942:content/blog/creating-custom-field-types.md
    function get_field_def(){
        $def = parent::get_field_def();
        $def['dbType'] = 'varchar';
        return $def;
    }
}
```

Next we create the language file
`custom/Extensionmodules/ModuleBuilder/Ext/Language/en_us.ColourPicker.php`
and define the label for our new field type:

<<<<<<< HEAD:content/community/future-blog-posts/5.creating-custom-field-types.adoc
.Example
[source,php]
=======
```php
>>>>>>> 16d81cf7aa486cd983f69bd15ee5acf2eb4c8942:content/blog/creating-custom-field-types.md
<?php
$mod_strings['fieldTypes']['ColourPicker'] = 'Colour Picker';
```

After a quick repair and rebuild this gives us the option to create a
field with type `Colour Picker`:

![ColourPickerStudio](/images/en/community/03ColourPickerStudio.png)

This can then be added to views and the like through studio in the usual
manner.

However our field is pretty boring and doesn’t do anything yet. Let’s
give it some personality.

We’ll use [TinyColorPicker](http://www.dematte.at/tinyColorPicker/) to add
some functionality to the field. This will get saved in a new directory
in
`custom/include/SugarFields/Fields/ColourPicker/js/jqColorPicker.min.js`

Next we’ll add two templates, one for the Detail view at
`custom/include/SugarFields/Fields/ColourPicker/DetailView.tpl`:

<<<<<<< HEAD:content/community/future-blog-posts/5.creating-custom-field-types.adoc
.Example
[source,php]
=======
```php
>>>>>>> 16d81cf7aa486cd983f69bd15ee5acf2eb4c8942:content/blog/creating-custom-field-types.md
<script src="custom/include/SugarFields/Fields/ColourPicker/js/jqColorPicker.min.js"></script>
{if strlen({{sugarvar key='value' string=true}}) <= 0}
    {assign var="value" value={{sugarvar key='default_value' string=true}} }
{else}
    {assign var="value" value={{sugarvar key='value' string=true}} }
{/if}
<input disabled="disabled" type='text' name='{{if empty($displayParams.idName)}}{{sugarvar key='name'}}{{else}}{{$displayParams.idName}}{{/if}}'
       id='{{if empty($displayParams.idName)}}{{sugarvar key='name'}}{{else}}{{$displayParams.idName}}{{/if}}'
       size='{{$displayParams.size|default:30}}'
       {{if isset($displayParams.maxlength)}}maxlength='{{$displayParams.maxlength}}'{{elseif isset($vardef.len)}}maxlength='{{$vardef.len}}'{{/if}}
       value='{$value}' title='{{$vardef.help}}' {{if !empty($tabindex)}} tabindex='{{$tabindex}}' {{/if}}
        {{if !empty($displayParams.accesskey)}} accesskey='{{$displayParams.accesskey}}' {{/if}}
        {{$displayParams.field}}>
                            //
<script>
    {literal}
    $(document).ready(function(){
        $('#{/literal}{{if empty($displayParams.idName)}}{{sugarvar key='name'}}{{else}}
        {{$displayParams.idName}}{{/if}}{literal}').colorPicker();
    });
    {/literal}
</script>
```

and one for the Edit View at
`custom/include/SugarFields/Fields/ColourPicker/EditView.tpl`:

<<<<<<< HEAD:content/community/future-blog-posts/5.creating-custom-field-types.adoc
.Example
[source,php]
=======
```php
>>>>>>> 16d81cf7aa486cd983f69bd15ee5acf2eb4c8942:content/blog/creating-custom-field-types.md
<script src="custom/include/SugarFields/Fields/ColourPicker/js/jqColorPicker.min.js"></script>
{if strlen({{sugarvar key='value' string=true}}) <= 0}
    {assign var="value" value={{sugarvar key='default_value' string=true}} }
{else}
    {assign var="value" value={{sugarvar key='value' string=true}} }
{/if}
<input type='text' name='{{if empty($displayParams.idName)}}{{sugarvar key='name'}}{{else}}{{$displayParams.idName}}{{/if}}'
       id='{{if empty($displayParams.idName)}}{{sugarvar key='name'}}{{else}}{{$displayParams.idName}}{{/if}}'
       size='{{$displayParams.size|default:30}}'
       {{if isset($displayParams.maxlength)}}maxlength='{{$displayParams.maxlength}}'{{elseif isset($vardef.len)}}maxlength='{{$vardef.len}}'{{/if}}
       value='{$value}' title='{{$vardef.help}}' {{if !empty($tabindex)}} tabindex='{{$tabindex}}' {{/if}}
        {{if !empty($displayParams.accesskey)}} accesskey='{{$displayParams.accesskey}}' {{/if}}
        {{$displayParams.field}}>
                            //
<script>
    {literal}
    $(document).ready(function(){
        $('#{/literal}{{if empty($displayParams.idName)}}{{sugarvar key='name'}}{{else}}
        {{$displayParams.idName}}{{/if}}{literal}').colorPicker();
    });
    {/literal}
</script>
```

After a quick repair and rebuild (and assuming the field has been added
to the views). You’ll see the new field:

![Colour Picker Exmaple](/images/en/community/04ColourPicker.png)
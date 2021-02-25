# Handlemarj - Preview MJML+Handlebars Templates
This is a simple extension of the [Handlebars Preview plugin](https://github.com/johnknoop/vscode-handlebars-preview) that performs the additional step of an MJML to HTML transform after the rendering of an HB template (HB->MJML->HTML).

This is a convenient way to use the power of Handlebars features in an MJML workflow - great for data-driven HTML email production and the use of partials to avoid duplication of common email elements.

**Requirements**\
You will also need the MJML VS Code extension installed for .mjml extension support, syntax highlighting etc.

**How to use**\
Open an MJML+Handlebars file (with .mjml extension), select **Handlemarj: Preview** from the command menu **or** right click on the editor tab. This will show you a preview of the HTML email.

## Example
Here's a simple example using a Handlebars partial to take care of the common layout parts of an email. The partials path is relative to your VS Code workspace root. If the below template is named `invoice.mjml` then you can supply preview data in a JSON file named `invoice.mjml.json`

```handlebars
{#> templates/partials/layout }}
    <mj-section>
        <mj-column>
            <mj-text align="left" padding="10px 25px">
                <h1>Your Tax Invoice</h1>
            </mj-text>
            <mj-text>
                <h3>{{invoice.reference}}</h3>
            </mj-text>
        </mj-column>
    </mj-section>
{{/templates/partials/layout}}
```

# Features (Taken Directly From Handlebars Preview)

✅ Image support\
✅ [Automatically scans your workspace folder(s) for partials and helpers](#partials)\
✅ [Auto-refresh](#auto-refresh)\
✅ [Generate context file from a template](#generate-context-file-from-template)

## Partials
Partials are automatically discovered and given names based off of the workspace folder root. So if these are the subfolders of the folder you've opened in VS Code:
```
.
└── 📁partials
    ├── 📁style
    │    └── dark.hbs
    └── footer.hbs
```
Then the two partials will be registered as `partials/footer` and `partials/style/dark` respectively.

## Helpers
Helpers can be defined as javascript modules and will be automatically discovered and registered. Helpers can be placed anywhere in the Workspace, as long as they use the double file extentions like: `.hbs.js` or `.handlebars.js`.
   
As an example, a typical helper file could look like this:
```js
// my_helpers.handlebars.js

module.exports = {
    toUpperCase: function (text) {
        return text.toUpperCase();
    },
    toLowerCase: function (text) {
        return text.toLowerCase();
    }
};
```
And could be used like this inside your Handlebars template to properly cast the `title` variable to upperCase:

```hbs
{{toUpperCase title}}
```

## Auto-refresh
Changes to Handlebars templates applied in real-time. Included partials need to be saved in order for the change to take effect.

## Generate context file from template
Right-click on a handlebars file in the sidebar or on the editor tab and select **Handlebars: Generate context file**.

A new file named `{yourfile}.json` will be created and populated with sample data.

> #### Current limitations of context generation:
> 🙁 [Block parameters](https://handlebarsjs.com/guide/block-helpers.html#block-parameters) in `each`-constructs are not supported\
> 🙁 Path segments (`../`) are currently not supported.
> 
> If you're using any of these features in your template, the resulting json will need some manual fixing.
> 
> Feel free to make a pull request if these limitations are bothering you.

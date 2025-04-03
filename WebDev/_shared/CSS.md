Cascading Style Sheets (CSS)

Cascading Style Sheets (CSS) is used to determine _how_ the HTML is displayed in the browser.

CSS is made up of the **Selector** and various **declarations**.

The selector links to the HTML tag, and the declaration is setting a particular property a value. Each declaration has to be ended by a semi-colon - `;`.

![[cssSelector.gif]]

## Example CSS

This CSS would set all `<p>` tags to be red, centred text.

```css
p {
  color: red;
  text-align: center;
}
```

# How to include CSS in your page/site.

There are three ways to include CSS:

- External CSS
- Internal CSS, and
- Inline

Each level of CSS gets more specific as you go from External to Inline.

> [!info] It’s also important to note that CSS is cumulative. Meaning that you can have external, internal and inline css all impacting HTML tags, and all of the CSS declarations will be applied.

## External CSS

External CSS is creating a seperate file to contain site-wide CSS. This is useful because then all pages can have the same CSS. When the CSS is changed in the external file, the whole site is updated.

### How to - External CSS

In Webstorms, right-click on the project folder, choose New and then Stylesheet.

![[cssCreateFile.png]]

Name the file appropriately.

![[cssNgunnawal.png]]

Create your first entry with the `h1` selector. Save the file.

> [!info] You can set the font-size or many other properties to anything that is possible. This site is good for a start: [https://www.w3schools.com/css/default.asp](https://www.w3schools.com/css/default.asp)


![[cssExternalCSS.png]]

```css
h1 {
	font-size: xxx-large;
}
```

Open your HTML file. Update the `<head>` section to include a link to the CSS file you created. This uses the `<link>` tag.

> [!info] The `rel` attribute indicates the type of relationship between the current HTML and the external file being loaded.


![[cssLinkStyleSheet.png]]

View the HTML page in your browser. You should notice that any `h1` tags are now show the text larger.

![[cssResult.png]]

Now that the connection has been made, you can edit the CSS and the page in the browser will update on refresh.

![[cssDemoChange.gif]]

Update all your HTML files with the same `<link>` tag in the `<head>` tag. This will make all pages use the same CSS and assist in making the site more consistent.

## Internal CSS

Internal CSS is CSS defined in the HTML file. The CSS selectors and declarations are **only** accessible from within that single HTML.

This uses the `<style> ... </style>` tag in the `<head>` section of the HTML.

### How To - Internal CSS

Within the `<head>` section, create a `<style> ... </style>` and include the CSS that you want.

> [!info] The syntax for Internal CSS is exactly the same as External CSS.


![[cssInternalCSS.png]]


```html
<style>
		h2 {
			color:blue;
		}
</style>
```

Load the page in the browser and you should see the changes made to the `<h2>` tag.

![[cssRedStyle.png]]

## Inline CSS

Inline CSS impacts only one HTML tag. This is used if you wish to change the way only one tag is displayed, and not the other sections of the HTML of the same tag. There are other ways to do this as well, but for simple changes, this can be very effective.

Find the tag you want to update and add a style attribute in the opening tag. Inside the quotes, enter the CSS you wish.

![[cssInlineCSS.png]]

```css
style="color:green;"
```


Load the page in the browser, and you’ll notice that only one paragraph (in this case) has been changed to green.

![[cssSelectors.png]]
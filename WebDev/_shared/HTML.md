HyperText Markup Language (HTML) is the language used to create the content and structure of a webpage.

![[htmlIntroduction.jpg]]

![https://www.youtube.com/watch?v=9p-YLfGWC68&ab_channel=CodeHS](https://www.youtube.com/watch?v=9p-YLfGWC68&ab_channel=CodeHS)

HTML is made up of Tags.

![[htmlStructure.png]]

# **HTML Tags**

There a a multitude of HTML tags that you can use, here is a small sample.

![[htmlTagsCheetSheet.png]]

In each of these sections, there are a number of **tags** that make up the sections. Here are some examples.

![[htmlExample.jpg]]

# HTML Example

In Webstorm, when you create a new HTML5 file, the default code is generated. This is a good basic structure for a HTML file. It shows the overall `<html>` tags as well as the `<head>` and `<body>` sections.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
</html>
```

To practice, this will show how to use some of the more common HTML tags that will be used.

## Titles - `<title>`

Change the text between the `<title>` tags to what you want the title to be.

Example: `<title>Ngunnawal Country</title>`

## Paragraphs - `<p>`

Blocks of text can be defined using the `<p> .. </p>` tag.

Everything between the `<p> .. </p>` will be displayed as a single paragraph.

Example:

```html
<p>
    Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Phasellus dolor lorem, lacinia vitae maximus aliquet, tristique at dui. Phasellus quis blandit sapien, eu pellentesque lorem. Donec ac augue in massa ullamcorper faucibus vel ac leo. Morbi vitae eros justo. Etiam ultrices malesuada commodo. Etiam porta fermentum neque vel molestie. Donec pharetra, arcu quis bibendum vestibulum, est velit sollicitudin urna, eu commodo metus risus vitae massa. Duis eros magna, pharetra et diam maximus, commodo pretium dolor. Nam vehicula ultrices lorem non ultricies. Vestibulum magna sapien, volutpat ac magna accumsan, aliquet finibus metus.
</p>
```

![[htmlParagraph.png]]

## Images - `<img>`

Images can be included in the HTML. It is best to include the images inside the project, preferably inside a folder/directory. The path to the image file has to match EXACTLY.

> [!info] `<img>` tags are one of the few tags that do not require a close (`</img>`) tag.


![[htmlImagesFolder.png]]

Example:

```html
<img src="images/ngunnawalsunset.jpeg">
```

## Links - `<a>`

Links to other HTML pages in the project, or to external website are done through the anchor - `<a>` - tag. As an attribute you need to define the link to the page. You also need to define what text the link appears as between the `<a>` and `</a>` tags.

Example:

```html
<a href="index.html">Home</a>
```

# HTML Layout

HTML can be used to determine the layout of the page in the browser. In “The Olden Days” (1990s) the `<table>` tag was used as the primary method for laying out content in the page. The developer could create rows and columns and put content (images or text etc) inside each “cell”.

However this is not the most current or suggested use.

## Reasons for not using Tables.

- Tables are not accessible (easy to sue for users with additional needs). Screen readers for people with sight issues can not correctly navigate tables and have them read out the content to the user.
- For complex websites, tables can get very complicated quickly.
- Slow to Load. The whole table needs to be read by the browser before it can display the content.
- Inflexible. Different sized screen widths can vastly impact the readability of tables. For example, mobile phones.

The `<div>` tag is your friend.

## The `<div>` Tag

The `<div>` tag is used to define a section of the website, and is referred to as a **container**. This means that it doesn’t display information by itself, but holds other information to display. You can include any other tags within `<div>` tags, even other `<div>`s.

> [!info] `<div>` stands for division. As in the tag defines a division or section of the website.

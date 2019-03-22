---
layout: post
title: How to Escape Greater Than (>) and Less Than (<) Sign in XSL Script
date: 2019-02-01
tags: [js, javascript, css, xsl, script]
mark: star
comments: true
---

> This is the solution to embed `>` and `<` in `Javascript` of `XSL` file

I am feeling sick of embedding `Javascript` code in `xsl`, particularity working with `>` and `<`. All in All, I do not know why `xsl` deals with `<script>` and `<xsl:script>` especially.  If using `<![CDATA[]]> `，`<xsl:text disable-output-escaping="yes">`,  `>` and `>` will output as `&gt;` and `&lt;` or throw an error. If using `&gt;` and `&lt;`, it outut `&gt;`, `&lt;` exactly the same, which is special only (or at leaat) in between `<script>` or `<xsl:script>`. Having failed googling, eventually I found the solution in our project: escaping the `<` and `>` signs for `<script>` tag instead of escaping in Javascript code for them.

### Javascipt `<script>`

1. Js Template
```xsl
<xsl:template name="javascript">
	<xsl:param name="code"/>
	<xsl:text disable-output-escaping="yes">
		&lt;script type="text/javascript"&gt;
		/* &lt;![CDATA[ */ </xsl:text>
		<xsl:value-of select="$code" disable-output-escaping="yes"/>
		<xsl:text disable-output-escaping="yes">
		/* ]]&gt; */
		&lt;/script&gt;
	</xsl:text>
</xsl:template>
```

2. Call "JavaScript" Template
```xsl
<xsl:template match="/">
    <xsl:call-template name="javascript">
        <xsl:with-param name="code">
            <![CDATA[ 
            function loop(){
                for(let i=0; i++; i<10){
                    console.log(i)
                }
            }
            ]]>
        </xsl:with-param>
    </xsl:call-template>
</xsl:template>
```

Output from brower:

![xsl输出的js代码](https://img-blog.csdnimg.cn/20190228014403872.jpg)

### CSS `<style>`
Thus, we can do the same thing on embedding css code.

```markup
<xsl:template name="style">
		<xsl:text disable-output-escaping="yes">
		&lt;style&gt;
		/* &lt;![CDATA[ */ </xsl:text>
		<xsl:text disable-output-escaping="yes">
			</xsl:text>
		<xsl:text disable-output-escaping="yes">
		/* ]]&gt; */
		&lt;/style&gt;
	</xsl:text>
</xsl:template>
```

>`xsl` is powerful, but some tricks are really painful.
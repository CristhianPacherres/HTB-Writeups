# HTB - OpenAdmin
Este es mi primer writeup asi como OpenAdmin(10.10.10.171) fue una de las primeras maquinas que resolvi. Espero que sea de ayuda y H4ppy H4ck1ng!
## Escaneando puertos
Utilizando el comando:
```markdown
nmap -sS --open -p- -sV -T 4 10.10.10.171 
```
![imagen](https://user-images.githubusercontent.com/84255799/119300072-c16ead80-bc25-11eb-9a3c-61bf6bc1ea3f.png)

Del escaneo, se puede observar que la maquina tiene 2 puertos abiertos, se requieren usuarios para el login de SSh por lo que se procede a enumerar el puerto 80.


You can use the [editor on GitHub](https://github.com/CristhianPacherres/HTB-Writeups/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

hola mundo2
c logro
vamos por mas
### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/CristhianPacherres/HTB-Writeups/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.

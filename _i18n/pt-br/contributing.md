## Estrutura do site

Este é um site multi-linguagem do Jekyll. Isso significa que você pode escrever alternativas para o seu texto em várias linguas! A lingua padrão deste tutorial é `pt-br` (Português Brasil).

Textos novos devem ser adicionados em `_i18n/<linguagem>/_posts` com o seguinte nome:

* **AAAA-MM-DD-titulo-sem-espacos.md**
  * **AAAA** => Ano com 4 digitos
  * **MM** => Mês com 2 digitos
  * **DD** => Dia com 2 digitos
  * **titulo-sem-espacos** => O título do seu texto sem espaços ou acentos

O arquivo deverá conter a seguinte estrutura básica:

```markdown
---
title: <Titulo da sua postagem, aqui pode acento e espaço!>
date: 2020-06-10T23:48:29-03:00
author: <Seu Nome>
translated-by: <Seu nome, caso esteja traduzindo um artigo>
layout: post
image: <Caminho para a imagem de apresentação>
tags:
  - <Tags para achar seu post>
---

Conteúdo
```

Onde:

* Campo delimitado por `---` é o **cabeçalho** onde estarão os metadados para construção da página.
  * **title** => Título da sua postagem. Aqui pode acento e espaço!
  * **date** => Data / Hora no formato ISO: `AAAA-MM-DDTHH:mm:ss-03:00`
    * **AAAA** => Ano com 4 digitos
    * **MM** => Mês com 2 digitos
    * **DD** => Dia com 2 digitos
    * **HH** => Hora com 2 digitos
    * **mm** => Minuto com 2 digitos
    * **ss** => Segundos com 2 digitos
    * **-03:00** => Fuso horário (neste caso, GMT-3)
  * **author** => Nome do(s) autor(es) do post
  * **translated-by** => Nome da pessoas que traduziram -- *Campo opcional*
  * **layout** => Layout da pagina (use sempre `post`!)
  * **image** => Uma imagem para apresentação do seu post  -- *Campo opcional*
  * **tags** => Lista de palavras chave para ajudar a encontrarem seu post pela internet! -- *Campo opcional*
* **Conteúdo** => Conteúdo Markdown / HTML do seu post


### Imagens

Caso deseja adicionar alguma imagem a sua publicação, coloque-a em `assets/posts/<titulo do seu post>` e você poderá ligar ela usando markdown ou html:

##### Markdown
```markdown
![Descrição da Imagem](/assets/posts/<titulo do seu post>/<sua imagem>.jpg)
```

##### HTML
```html
<img
  src="/assets/posts/<titulo do seu post>/<sua imagem>.jpg"
  alt="Descrição da sua imagem"
/>
```

## Testando o site localmente

Este é um site [Jekyll](https://jekyllrb.com/)! Todas as instruções normais do jekyll se aplicam para este tutorial. Para rodar localmente em seu computador é nescessário ter o [Ruby](https://www.ruby-lang.org/pt/) instalado (o que é já existe por padrão na maioria das distribuições Linux e MacOSX).

```bash
# Instalar Bundler e o Jekyll
gem install bundler jekyll

# Clonar o repositorio
git clone https://github.com/racerxdl/sat4noobs
cd sat4noobs

# Instalar dependências
bundle install

# Rodar o site
bundle exec jekyll serve --host 0.0.0.0 --watch
```

Feito isso, deve ser apresentado uma mensagem dizendo que o site está pronto para acesso:

```
Server address: http://0.0.0.0:4000
```

O site será acessível no seu browser pelo endereço [http://localhost:4000](http://localhost:4000)

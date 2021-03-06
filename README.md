# Middleman Paginator
A simple plugin that allows paginating any collection of objects

## Instalation

Add this to your Gemfile

```
gem 'middleman-paginator'
```

## Use

### Activate gem in `config.rb`

```ruby
  activate :paginator do |e|
    e.per_page = 25 # Default 20
  end
```

### Paginate

`destination` - Path to put pages in. Expects `/` in the end.
Produced pages will have following path `#{destination}pages/index.html`, `#{destination}pages/2.html`

`template` - Path to the template.
If you don't want to render template by itself pass `ignore: true` to extra arguments.

`objects` - Objects to paginate. You will have access to them inside the template as `objects`.

`locals` -  If you have need any other variables inside the template, pass them allong as you normally would.

`ignore` - See `template` option description.

```ruby
paginate(
  destination: '/',
  template: 'template.html',
  objects: objects,
  locals: { },
  ignore: true
)
```

### Access from template

Inside `template`, iterate over `per_page` number of `objects`

```haml
  - paginator[:objects].each do |object|
    // render object 

  = previous_page_link
  = next_page_link

```

As you might've noticed paginator injects local variable called `paginator`. With following helpful data: `objects`, `page`, `destination`, `last_page`

### Helpers

Paginator provides a few helper methods:
  * `last_page_link`
  * `previous_page_link`
  * `next_page_link`
  * `last_page_link`
  * `pages`
They work like `link_to` helper and support same the arguments.

Here is a haml example with bootstrap styles:

```haml
- if pages.count > 1
  %ul.pagination
    - prev_link = previous_page_link text: '<', class: 'page-link'
    - next_link = next_page_link text: '>', class: 'page-link'

    %li.page-item{ class: ('disabled' if prev_link.blank?) }
      - if prev_link
        = prev_link
      - else
        .page-link <

    - pages do |page, destination|
      %li.page-item{ class: ('active' if page == paginator[:page]) }
        = link_to page, destination, class: 'page-link'

    %li.page-item{ class: ('disabled' if next_link.blank?) }
      - if next_link
        = next_link
      - else
        .page-link >
```

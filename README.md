# Non-React JS


## Sample javascript structure

- This is an example for a page specific javascript.

``` javascript
// project_new.js
$(
    // Name the object(class) as a combination of resource and action
    var ProjectNew = {
        init: function() {
            // memoise the required DOM elements. Do Parse the DOM multiple times 
            var form = $("#project-new");
            var inputs = $(".js-project-user-fields");

            // memoise the DOM required across multiple functions
            this.userRole = $('.ajax-project-select');

            // Breakdown the logic into logical functions. Invoke them as required.
            this.initVisibilitySelect();
            // These act as private methods
            this.toggleSettings();
            this.toggleSettingsOnclick();
            this.toggleRepoVisibility();
        },

        initVisibilitySelect: function(){
            //...
        },

        toggleSettings: function() {
            //....
        },
    }

    ProjectNew.init();
);
```

- In the corresponding view, call the javascript file using `content_for`

``` ruby
<% content_for(:body_end) do %>
  <%= javascript_include_tag('project_new') %>
<% end %>
```

- If you have multiple JS files for the same page, create a new manifest file and include that in the `content_for`.

``` ruby
<% content_for(:body_end) do %>
  <%= javascript_include_tag('project_new_common') %>
<% end %>
```

Make sure you use `body_end`, so that the javascript files are loaded after the DOM is rendered. Try to avoid loading them in header `content_for(:scripts)`.

## Passing values from views to javascript.

- If the value to be shared is just a simple `id` or `name`, embed it in data attribute of the html element.

```
<% =link_to body, url, { data: { foo: "bar" } } %>
```

Then access it in javascript

```
$("a").data("foo");
```

- If there it is a complex data structure, you can store it in the global window object.

```
<%= javascript_tag do %>
  window.products = <%= @products.to_json %>;
<% end %>
```

Make sure you do not override other variables when storing values in gloabl namespace.
It is always safe to create a namespaces object and then populate the values

```
<%= javascript_tag do %>
  window.approvals = {};
  window.approvals = <%= @products.to_json %>;
<% end %>
```

## Some pointers
- Do not abuse document.ready() or the shortcut $(function(){}). Make sure you have just one per page.
- You do not need jQuery for everything. You can always use `document.querySelector` and other vanilla javascript methods.
- Use `.on` with `click` as the argument over `.click`



 
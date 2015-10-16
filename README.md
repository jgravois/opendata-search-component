# opendata-search component

## Installation
`npm install opendata-search-component`

## Usage

Examples can be seen [here](http://arcgis.github.io/opendata-search-component/examples/).

1. Include the [opendata-search.js](https://github.com/ArcGIS/opendata-search-component/tree/master/dist) script _before_ you make use of the element in the DOM.
1. Drop the element on the page as you would any other HTML element. `<opendata-search></opendata-search>`

### Options

Options can be specified as attributes on the element.
````
<opendata-search
  api="http://my.opendata.url"
  limit="20"
  sort="relevance"
  group="my_group_id"
  fields="record_count, item_type"></opendata-search>
````

* `api` : Specifies the api url to search. Default: http://opendata.arcgis.com.
* `limit` : Specifies how many results to return. Default: api default, currently 10.
* `sort` : Specifies how the results should be sorted. Currently the possible values are `created_at`, `updated_at`, `relevance`. Default: api default, currently `updated_at`.
* `group` : Specifies the group id in which to search. Default: all groups exposed in the api.
* `fields` : Specifies the fields to request from the api. It is not necessary to supply this unless you provide a custom result item template (see below) that uses fields not used by default. Note that this is not currently supported - the api always returns all fields.

### Customizing the display

The display of the component can be styled with CSS just as you would style any other HTML element.

If you need control of the actual markup generated by the component or you need to provide alternative languages, you can provide markup inside the element tags. For example, if you use bootstrap, you might do something like this:

````
<opendata-search id="my-od-search">
  <form>
    <div class="form-group">
      <div class="input-group">
        <input type="search" class="form-control input-lg" placeholder="Search for open data">
        <span class="input-group-btn">
          <button class="btn btn-default btn-lg" type="submit">
            <span class="glyphicon glyphicon-search" aria-hidden="true"></span>
          </button>
        </span>
      </div>
    </div>
  </form>
</opendata-search>
````

The component depends on an `input` inside a `form`. If the provided markup does not contain those elements, the component will add them. The component will display the search results inside an element with the class `od-search-results`. If the provided markup does not contain that element, the component will add it. The component will not remove any markup provided inside its opening and closing tags.

To control the display of the search results, provide a template like this:

````
<opendata-search fields="item_type">
  <script id="od_result_item_template" type="text/template">
    <li class="od-search-results-item">
      <h1>
        <a href="<%=dataset_url%>" target="_blank">
          <%=name%>
        </a>
      </h1>
      <small>(<%=item_type%>)</small>
    </li>
  </script>
</opendata-search>
````

Note that the template must be inside script tags with `type="text/template"` and `id="od_result_item_template"`. Tokens to replace with search result item properties should be specified with tags like: `<%=property_name%>`. Note that if you make use of search result item properties not used by the component by default, you should specify them on the `fields` attribute of the component.

The "no results" message can be similarly customized by providing a template with `id="od_no_results_template"`. This template has access to the properties of the element, so you could do something like:

````
<opendata-search>
  <script id="od_no_results_template" type="text/template">
    <div>We couldn't find any datasets matching '<%=q%>' at '<%=api%>'</div>
  </script>
</opendata-search>
````

### With a framework

#### React

Currently React 0.13 uses a [whitelist of valid HTML attributes](https://github.com/facebook/react/issues/140) so you cannot use non-standard attributes in React. This will be fixed for custom elements with React 0.14. With this change custom elements should work totally out of the box.

#### Ember

Currently in addition to [wrapping the custom element as a component](examples/ember-app/app/components/item-rating.js) you must [register any custom events with Ember](examples/ember-app/config/environment.js#L16-L23). Eventually the component restriction will go away with the move to angle bracket components in Ember 2.2, see the [RFC](https://github.com/emberjs/rfcs/pull/60) and the [PR](https://github.com/emberjs/ember.js/pull/12011).

The restriction to add custom event to the global registry will still remain. However the author of a proposed event [refactoring RFC](https://github.com/emberjs/rfcs/pull/86) is interested in removing the requirement.

#### Angular 2.0

Works totally out of the box.

#### Angular 1.0

Attribute binding works fine out of the box but in order to bind custom events you must [write a small custom directive](/examples/angular-1-app/index.html#L28-L44).

#### Backbone

Works totally out of the box.

#### Aurelia

Works totally out of the box, as long as you obey the [DOM standards](#strictly-adhere-to-dom-standards), particularly with regards to properties and attributes. If you want to break standards for more readable attribute names or some other reason you can extend Aurelia's parser to support your use case.

## Browser support

Works in all modern browsers back to IE 9.

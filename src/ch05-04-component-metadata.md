## Component Metadata

According to the official documentation, you can use the following set of properties with a `@Component` decorator:

| Name | Type | Description |
| --- | --- | --- |
| changeDetection | ChangeDetectionStrategy | Defines the change detection strategy the component should use. |
| viewProviders | Provider[] | Defines the set of injectable objects that are visible to its view DOM children. |
| moduleId | string | ES/CommonJS module id of the file in which this component is defined. |
| templateUrl | string | Specifies a URL containing a relative path to an external file containing a template for the view. |
| template | string | An inline-defined template for the view. |
| styleUrls | string[] | List of URLs containing relative paths to the stylesheets to apply to this component's view at runtime. |
| styles | string[] | List of inline-defined styles to apply to this component's view at runtime. |
| animations | any[] | List of animations of this component in a special DSL-like format. |
| encapsulation | ViewEncapsulation | Defines style encapsulation strategy used by this component. |
| interpolation | [string, string] | Overrides the default encapsulation start and end delimiters (respectively `{{` and `}}`). |
| entryComponents | Array<Type\<any> \| any[]> | List of components that are dynamically inserted into the view of this component.|

The `@Component` decorator extends the `@Directive` one, so it also inherits the following set of properties you can use as well:

| Name | Type | Description |
| --- | --- | --- |
| selector | string | CSS selector that identifies this component in a template. |
| inputs | string[] | List of class property names to data-bind as component inputs. |
| outputs | string[] | List of class property names that expose output events that others can subscribe to. |
| host | { [key: string]: string; } | Map that specifies the events, actions, properties and attributes related to the host element. |
| providers | Provider[] | List of providers available to this component and its children. |
| exportAs | string | A name under which the component instance is exported in a template. |
| queries | { [key: string]: any; } | Map of queries that can be injected into the component. |

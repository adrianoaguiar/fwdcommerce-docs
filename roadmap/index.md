# Development Roadmap

The following items are listed in no particular order. If you'd like to contribute or have anything to add, you may submit a <a href="https://github.com/getfwd/fwdcommerce-docs">pull request</a>, post in the <a href="/alpha/forums">forums</a>, or e-mail <a href="mailto:feedback@getfwd.com">feedback@getfwd.com</a>.

### Settings Management <span class="label label-info">Planning</span>

Admin interface for managing settings (the ones currently found in config/app.yml).

### Language Localization <span class="label label-info">Planning</span>

A system and core helper for localizing language text. We are considering support for various file formats such as PO and YAML. If you have a preference, we'd love to hear from you.

### Custom Attribute Management <span class="label label-info">Planning</span>

Although collection attributes can be added on the fly already (thanks to MongoDB), not everyone wants to edit Admin HTML to do it. This feature will make it possible to add custom attributes to any collection without writing code.

### Product Configuration <span class="label label-info">Planning</span>

A product configuration interface for creating "product types", with the ability to specify properties and attributes for each type.

### Inventory Tracking <span class="label label-info">Planning</span>

A new model for tracking inventory quantities associated with products and variants. Templates can use this information to display inventory levels and trigger actions based on available stock.

### Unit/Integration Test Suite <span class="label label-info">Planning</span>

A robust test suite to ensure APIs and core classes do what they are meant to do, and developers can move faster knowing their plugins and custom code won't break.

### Payment Gateways <span class="label label-info">Planning</span>

A core payment plugin with combined support for Stripe, PayPal, Authorize.Net, and others. It will be enhanced continually to support additional payment gateways over time. Note: Core support already exists for Stripe.

### Shipping Carriers <span class="label label-info">Planning</span>

A core shipping plugin with combined support for UPS, USPS, FedEx, and other international carriers. It will be enhanced continually to support additional shipping carriers over time. Note: Core support already exists for UPS and FedEx.

### Custom Reporting <span class="label label-info">Planning</span>

Admin interface for custom reporting. It will allow you to create and manage custom reports based on any collection with variable algorithms.

### Discount Types <span class="label label-info">Planning</span>

Additional discount types including those applied by product catalog rules and account roles.

### Currency/Unit Conversion <span class="label label-info">Planning</span>

Support for unit types and conversion (currency, weight, length). Real-time currency conversion rates are to be implemented by plugin.

### Template/Plugin Management <span class="label label-info">Planning</span>

Admin interface for managing the installation and settings of templates and plugins.

### Template Themes <span class="label label-info">Planning</span>

A set of conventions for creating templates that support "themes". A theme is mean to replace the user interface portion of a template without affecting the functional behavior of that template. Alpha and Admin templates will be updated to support theming.

### Template Assets <span class="label label-info">Planning</span>

A convention that allows static assets to be contained and served directly from template directories (app/templates/.../assets/). It will reduce the work needed to distribute and install templates.

### MongoDB Management <span class="label label-info">Planning</span>

Admin interface for managing MongoDB connections, collections, documents, and indexes. Think of it as a lightweight phpMyAdmin for Mongo.

### Improved Channels <span class="label label-info">Planning</span>

Enhancements to the Admin channel management interface, including: ability to specify which field to use as list Title, Description, and Labels, ability to specify which fields are searchable, ability to drag/drop sort fields, and general improvement to the Manage Channels interface.

### Bootstrap Upgrade <span class="label label-info">Planning</span>

The Admin template will be upgraded to the latest version of <a href="http://twitter.github.com/bootstrap/">Twitter Bootstrap</a>.

### Bug Fixes

Various bugs will be addressed in every release, according to the list of <a href="https://github.com/getfwd/fwdcommerce/issues">Issues in github</a>.



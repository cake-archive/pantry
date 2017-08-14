# Cake Pantry

A collection of simple Handlebars templates that can be used to generate resources or templates in a variety of formats.

Note that since the majority of the expressions are **not** HTML-escaped (i.e. using `{{{ }}}`), these templates should not be used in an untrusted context.

## Data Model

The templates include Handlebars data placeholders (i.e. `{{ variable }}`) for parts of the files that are likely to require changes. While some properties are provided with sane defaults in the templates, most are required to be passed in during the rendering process.

The model in use is shared across all files so that the same model can be used across all templates. This model is described as a JSON schema in the `template.schema.json` file, and a sample JSON structure can be found in `templateData.json`

Note that the one exception is the Frosting task template which takes a singular task object (i.e. `/properties/cake/properties/frosting/properties/tasks/items` in the schema) as it can be generated multiple times.

### Frosting

Since it's a little more complicated, Frosting templates require a bit of forethought. This is most evident with generating Tasks: to use a custom context (i.e. inherit from `FrostingTask<TContext>` etc), you'll need to use the `template/frosting/Tasks/Tasks.cs.hbs` file which generates all tasks in the same file. To get separate files, use the `template/frosting/Tasks/Task.cs.hbs` file which takes a single `task` model (as above) and generates a single task implementation, but can only inherit from `FrostingTask`.

## Consuming projects

These templates can be used by any tool to generate either finalised resources or tool-specific templates.

While still in development, these projects (and their update mechanisms) are included below:

### Cake.Yeoman

The [`cake-build/cake-yeoman`](https://github.com/cake-build/cake-yeoman) project is the first to be cut over to Pantry templates. Template generation is handled by a separate `templates.cake` file, which can be invoked from the bootstrappers using `build.sh --build-templates` or `build.ps1 -BuildTemplates`, or directly using `cake templates.cake`.
# maquette-mapper

[![Greenkeeper badge](https://badges.greenkeeper.io/nextorigin/maquette-mapper.svg)](https://greenkeeper.io/)

[![Dependency Status][dependency]][david]
[![devDependency Status][dev-dependency]][david-dev]
[![Downloads][downloads]][npm]

Helper class to create MaquetteJS mappings from Models to Views

[![NPM][npm-stats]][npm]

## Installation
```sh
npm install --save-dev maquette-mapper
```

## Usage

#### Example: el-borracho-ui + gulp-pug-hyperscript

##### Model [filter.coffee](https://github.com/nextorigin/el-borracho-ui/blob/master/src/models/filter.coffee)
```coffee
class Filter extends Spine.Model
  @configure "Filter",
    "type",
    "value"
```

##### View [filter.jade](https://github.com/nextorigin/el-borracho-ui/blob/master/src/views/filter.jade)
```coffee
li(class="filter #{type}" id="filter-#{id}" key=id)
  h6 #{type}:
    span.value= value
  button.icon.delete(title="delete") Â
```

##### [filters.jade](https://github.com/nextorigin/el-borracho-ui/blob/master/src/views/filters.jade)
```coffee
ul!= filters()
```

##### Controller [filters.coffee](https://github.com/nextorigin/el-borracho-ui/blob/master/src/controllers/filters.coffee#L27)
```coffee
  constructor: ->
    super

    @Store      = require "../models/filter"
    @view       = require "../views/filters"
    @filterView = require "../views/filter"

    @projector or= Maquette.createProjector()
    @filterMap   = new Mapper [], @filterView

    @Store.on "error", @error
    @Store.on "change", @projector.scheduleRender
    @projector.append @el[0], @render

  render: =>
    @log "rendering"
    filters = @Store.all()

    @filterMap.update filters
    @view {filters: @filterMap.components}
```

## License

MIT

  [dependency]: https://img.shields.io/david/nextorigin/maquette-mapper.svg?style=flat-square
  [david]: https://david-dm.org/nextorigin/maquette-mapper
  [dev-dependency]: https://img.shields.io/david/dev/nextorigin/maquette-mapper.svg?style=flat-square
  [david-dev]: https://david-dm.org/nextorigin/maquette-mapper?type=dev
  [downloads]: https://img.shields.io/npm/dm/maquette-mapper.svg?style=flat-square
  [npm]: https://www.npmjs.org/package/maquette-mapper
  [npm-stats]: https://nodei.co/npm/maquette-mapper.png?downloads=true&downloadRank=true&stars=true

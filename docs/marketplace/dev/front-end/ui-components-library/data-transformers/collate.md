---
title: Data Transformer Collate
description: This document provides details about the Data Transformer Collate service in the Components Library.
template: concept-topic-template
---


This document provides details about the Data Transformer Collate service in the Components Library.

## Overview

Data Transformer Collate is an Angular Service that implements sorting, filtering, and pagination of data based on configuration.
In general, the meaning of the word `collate` is to collect, arrange and assemble in a specific order of sequence.

```html
<spy-table
  [config]="{
    datasource: {
      type: 'inline',
      data: {
        col1: '2020-09-24T15:20:08+02:00',
        col2: 'col 2',
      },                                                     
      transform: {
        type: 'collate',
        configurator: {
          type: 'table',
        },
        filter: {
          date: {
            type: 'range',
            propNames: 'col1',
          },
        },
        search: {
          type: 'text',
          propNames: ['col2'],
        },
        transformerByPropName: {
          col1: 'date',
        },  
      },
    },
  }"
></spy-table>
```

## Collate Filters

Collate Filters are Angular Services that extend filtering in the Data Transformer.
These services are registered via `CollateDataTransformer.withFilters()`.
There are a few common Data Transformer Collate Filters that are available as separate packages in the UI library:

  - `equals` - filters values that are strictly equal.
  - `range` - filters values that are within a number range.
  - `text` - filters values that match a string.

## Collate Data Configurators

Data Configurators are Angular Services that enable re-population of data (sorting, pagination, filtering).
These services are registered via `CollateDataTransformer.withConfigurators()`.
There are a few common Data Transformers Collate Data Configurators that are available:

  - `table` - integrates Table into Collate to re-populate data when the table updates.

## Interfaces

Below you can find interfaces for Data Transformer Collate.

### DataTransformerConfiguratorConfig
`configurator` - the object with the Data Transformer configurator type and additional properties.  
`filter` - the object based on a specific data property (`filterId`) that defines the properties on which the initial data object is filtered via `DataTransformerFilterConfig`.    
`search` - defines the properties on which the initial data object is filtered via `DataTransformerFilterConfig`.  
`transformerByPropName` - the object with data properties list that needs to be transformed.  

### DataTransformerConfiguratorConfig
`type` - the declared name of the module whose data needs to be transformed.  

### DataTransformerFilterConfig
`type` - the name of a filter, for example, `range`.  
`propNames` - the array with the property names to which the filter is applied.

```ts
export interface CollateDataTransformerConfig extends DataTransformerConfig {
  configurator: DataTransformerConfiguratorConfig;
  filter?: {
    [filterId: string]: DataTransformerFilterConfig;
  };
  search?: DataTransformerFilterConfig;
  transformerByPropName?: DataFilterTransformerByPropName;
}

export interface DataTransformerConfiguratorConfig {
  type: DataTransformerConfiguratorType;
  [prop: string]: unknown; // Extra configuration for specific types
}

export interface DataTransformerFilterConfig {
  type: string;
  propNames: string | string[];
}

export type DataFilterTransformerByPropName = Record<string, string>;

// Service registration
@NgModule({
  imports: [
    DataTransformerModule.withTransformers({
      collate: CollateDataTransformerService,
    }),

    // Filters
    CollateDataTransformer.withFilters({
      equals: EqualsDataTransformerFilterService,
      range: RangeDataTransformerFilterService,
      text: TextDataTransformerFilterService,
    }),

    // Configurators
    CollateDataTransformer.withConfigurators({
      table: TableDataTransformerConfiguratorService,
    }),
  ],
})
export class RootModule {}
```
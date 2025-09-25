# sapFluxR User Inputs Guide

This document provides a comprehensive guide to all user inputs required for the sapFluxR package, organised by category and function.

## Table of Contents

1. [Data Input](#data-input)
2. [Wood Properties](#wood-properties)
3. [Probe Properties](#probe-properties)
4. [Tree Measurements](#tree-measurements)
5. [Calculation Parameters](#calculation-parameters)

---

## Data Input

### Primary Data File
- **Variable**: `file_path`
- **Type**: Character string
- **Description**: Path to sap flow data file
- **Required**: Yes
- **Supported Formats**:
  - ICT Current format (JSON-like)
  - ICT Legacy format
  - CSV/Tab-delimited format
- **Example**: `"data/sap_flow_2024.txt"`

### Data Import Options
- **Variable**: `format`
- **Type**: Character string
- **Description**: Data format specification
- **Required**: No (auto-detected if NULL)
- **Options**: `"ict_current"`, `"ict_legacy"`, `"csv"`
- **Default**: Auto-detected

- **Variable**: `validate_data`
- **Type**: Logical
- **Description**: Whether to validate imported data
- **Required**: No
- **Default**: `TRUE`

- **Variable**: `chunk_size`
- **Type**: Integer
- **Description**: Characters per chunk for large files
- **Required**: No
- **Default**: Auto-sized based on file size

- **Variable**: `show_progress`
- **Type**: Logical
- **Description**: Show progress updates
- **Required**: No
- **Default**: `TRUE` for files >1MB

---

## Wood Properties

### Required Wood Properties
These properties are required for converting heat pulse velocity to sap flux density:

#### Fresh Wood Density
- **Variable**: `fresh_density`
- **Type**: Numeric
- **Description**: Fresh sapwood density
- **Units**: g/cm³
- **Range**: 0.3 - 1.2 g/cm³
- **Default**: 0.8 g/cm³ (typical hardwood)
- **Species Examples**:
  - Eucalyptus: 0.7-0.9 g/cm³
  - Pine: 0.4-0.6 g/cm³
  - Oak: 0.6-0.8 g/cm³

#### Dry Wood Density
- **Variable**: `dry_density`
- **Type**: Numeric
- **Description**: Dry wood density
- **Units**: g/cm³
- **Range**: 0.2 - 0.8 g/cm³
- **Default**: 0.55 g/cm³
- **Species Examples**:
  - Eucalyptus: 0.5-0.7 g/cm³
  - Pine: 0.3-0.5 g/cm³
  - Oak: 0.4-0.6 g/cm³

#### Moisture Content
- **Variable**: `moisture_content`
- **Type**: Numeric
- **Description**: Gravimetric moisture content
- **Units**: Decimal (0-1)
- **Range**: 0.0 - 1.0 (0-100%)
- **Default**: 0.45 (45%)
- **Typical Range**: 0.3-0.6 (30-60%)

#### Specific Heat Capacity
- **Variable**: `specific_heat_capacity`
- **Type**: Numeric
- **Description**: Specific heat capacity of dry wood
- **Units**: J/g/°C
- **Range**: 1.0 - 2.0 J/g/°C
- **Default**: 1.5 J/g/°C
- **Species Examples**:
  - Most hardwoods: 1.3-1.7 J/g/°C
  - Most softwoods: 1.4-1.8 J/g/°C

### Thermal Diffusivity
- **Variable**: `thermal_diffusivity`
- **Type**: Numeric
- **Description**: Thermal diffusivity of fresh sapwood
- **Units**: cm²/s
- **Range**: 0.001 - 0.01 cm²/s
- **Default**: 0.0025 cm²/s
- **Species Examples**:
  - Eucalyptus: 0.002-0.003 cm²/s
  - Pine: 0.0025-0.0035 cm²/s
  - Oak: 0.002-0.0025 cm²/s

---

## Probe Properties

### Probe Configuration
- **Variable**: `config_name`
- **Type**: Character string
- **Description**: Name of probe configuration
- **Options**: `"three_probe_symmetric"`, `"three_probe_asymmetric"`, `"four_probe_extended"`
- **Default**: Auto-detected

### Sensor Positions
- **Variable**: `sensor_positions`
- **Type**: Named list
- **Description**: Sensor positions relative to heater
- **Format**: `list(upstream = -6, downstream = 6)`
- **Units**: mm
- **Standard Configurations**:
  - Three-probe symmetric: upstream = -6, downstream = 6
  - Three-probe asymmetric: upstream = -5, downstream = 10
  - Four-probe extended: upstream = -6, downstream = 6, additional sensors

### Probe Diameter
- **Variable**: `probe_diameter`
- **Type**: Numeric
- **Description**: Probe diameter
- **Units**: mm
- **Range**: 1.0 - 2.0 mm
- **Default**: 1.27 mm (standard ICT probe)

### Heater Position
- **Variable**: `heater_position`
- **Type**: Numeric
- **Description**: Position of heater element
- **Units**: mm
- **Default**: 0 (center)

---

## Tree Measurements

### Required Measurements

#### Diameter at Breast Height (DBH)
- **Variable**: `diameter_breast_height`
- **Type**: Numeric
- **Description**: Diameter at breast height (1.3m)
- **Units**: cm
- **Range**: 5 - 200 cm
- **Required**: Yes
- **Typical Range**: 10-50 cm

### Optional Measurements

#### Bark Thickness
- **Variable**: `bark_thickness`
- **Type**: Numeric
- **Description**: Bark thickness
- **Units**: cm
- **Range**: 0.1 - 5.0 cm
- **Default**: Estimated from DBH
- **Estimation Methods**: Allometric relationships

#### Heartwood Dimensions
Choose one of the following:

##### Heartwood Radius
- **Variable**: `heartwood_radius`
- **Type**: Numeric
- **Description**: Heartwood radius
- **Units**: cm
- **Range**: 0 - (stem_radius - bark_thickness)
- **Default**: Estimated from DBH

##### Heartwood Diameter
- **Variable**: `heartwood_diameter`
- **Type**: Numeric
- **Description**: Heartwood diameter
- **Units**: cm
- **Range**: 0 - (stem_diameter - 2*bark_thickness)
- **Default**: Estimated from DBH

##### Sapwood Thickness
- **Variable**: `sapwood_thickness`
- **Type**: Numeric
- **Description**: Sapwood thickness
- **Units**: cm
- **Range**: 1.0 - 10.0 cm
- **Default**: Calculated from other measurements

### Estimation Parameters

#### Estimation Method
- **Variable**: `estimation_method`
- **Type**: Character string
- **Description**: Method for estimating missing parameters
- **Options**: `"allometric"`, `"conservative"`, `"species_specific"`
- **Default**: `"allometric"`

#### Species Group
- **Variable**: `species_group`
- **Type**: Character string
- **Description**: Species group for parameter estimation
- **Options**: `"hardwood"`, `"softwood"`, `"eucalyptus"`, `"pine"`
- **Default**: `"hardwood"`

---

## Input Validation Summary

### Required Inputs
1. **Data file path** - Primary sap flow data
2. **DBH** - Tree diameter at breast height
3. **Wood properties** - For flux density conversion (or use defaults)

### Recommended Inputs
1. **Species-specific wood properties** - For accurate results
2. **Probe configuration** - For method compatibility
3. **Custom parameters** - For specific sensor setups

### Optional Inputs
1. **Tree measurements** - Bark thickness, heartwood dimensions
2. **Quality control ranges** - Custom validation thresholds
3. **Export options** - Format and content preferences

### Default Values
The package provides sensible defaults for most parameters, but users should provide species-specific values for wood properties and thermal diffusivity for best results.



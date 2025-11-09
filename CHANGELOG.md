# Changelog

All notable changes to the Kapetanios Unit Root Test package will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.1] - 2025-11-09

### Fixed
- **Critical**: Fixed RecursionError caused by circular dependency between `_select_lags` and `_estimate_model` methods
- Fixed dimension mismatch in lagged difference construction that caused ValueError
- All lag selection methods (AIC, BIC, t-stat) now work correctly without recursion errors

### Changed
- Modified `_estimate_model` method signature to accept specific lag count (`k`) instead of maximum lags (`k_max`)
- Updated `_search_breaks` method to call `_select_lags` before `_estimate_model` to break circular dependency
- Improved lag construction logic to properly handle matrix dimensions and observation dropping
- Updated OLS estimation to use dimension-adjusted dependent variable

### Technical Details
- Separated concerns: `_select_lags` determines optimal k, `_estimate_model` estimates with specific k
- Properly drops initial observations when adding lagged differences to maintain dimension consistency
- All test suites pass successfully with the fix

## [1.0.0] - 2024-11-08

### Added
- Initial release of kapetanios-test package
- Implementation of Kapetanios (2005) unit root test with up to 5 structural breaks
- Three model types (A, B, C) for different break specifications
- Sequential search procedure following Bai & Perron (1998)
- Automatic lag selection using AIC, BIC, or t-statistic methods
- Critical values from Kapetanios (2005) Table I
- Comprehensive test suite with >20 unit tests
- Detailed documentation and README
- Example scripts demonstrating various use cases
- Windows, Linux, and macOS compatibility
- Support for NumPy arrays and Pandas Series input
- Approximate p-value calculation
- Type hints throughout codebase
- Proper error handling and warnings

### Features
- `KapetaniosTest` class for object-oriented interface
- `kapetanios_test()` convenience function
- `KapetaniosResult` dataclass for structured results
- Automatic adjustment of max_breaks based on trimming parameter
- Deterministic results for reproducibility
- Memory-efficient implementation
- Fast sequential search algorithm

### Documentation
- Complete README with examples and methodology
- Inline code documentation
- Type annotations
- Example scripts
- Test suite documentation

### Dependencies
- Python >= 3.8
- NumPy >= 1.20.0
- Pandas >= 1.3.0
- SciPy >= 1.7.0
- statsmodels >= 0.13.0

## [Unreleased]

### Planned Features
- Bootstrap p-values
- Confidence intervals for break dates
- Parallel computing support for simulations
- Additional diagnostic plots
- Integration with statsmodels
- GPU acceleration for large simulations
- Additional break date selection criteria

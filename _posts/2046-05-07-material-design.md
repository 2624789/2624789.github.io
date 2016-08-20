# How to build an App

Here are some generalized steps that may be used to conceptualize the application implementation process:

	1. Plan your layout and the components you want to use; preferrably with a site UX design document.
	2. Use hard-coded HTML and mock content to make sure the components render as desired
	3. Integrate UI components to your custom application logic. Use the seamless integration possible with Angular directives and controllers.This integration assumes that you have unit tested your app logic
	4. Add Adaptive breakpoints
	5. Add Theming support
	6. Confirm ARIA compliance
	7. Write e2e Tests. It is important to validate your app logic with Angular Material UI components.



	* Angular Material layout and flex options can easily configure HTML containers
	* Angular Material components <md-toolbar>, <md-sidenav>, <md-icon> can be quickly used
	* Custom controllers can use and show <md-bottomsheet> with HTML templates
	* Custom controller can easily, programmatically open & close the SideNav component.
	* Responsive breakpoints and $mdMedia are used
	* Theming can be altered/configured using $mdThemingProvider

# Diseño

Limit your selection of colors by choosing three color hues from the primary palette and one accent color  the secondary palette. The accent color may or may not need fallback options.

## Hues / Shades
A hue/shade is a single color within a palette.

## Palettes
A palette is a collection of hues. By default, Angular Material ships with all palettes from the material design spec built in. Valid palettes include:

## Color Intentions
A color intention is a mapping of a palette for a given intention within your application.

Valid color intentions in Angular Material include:

	* primary - used to represent primary interface elements for a user
	* accent - used to represent secondary interface elements for a user
	* warn - used to represent interface elements that the user should be careful of

## Themes
A theme is a configuration of palettes used for specific color intentions. A theme also specifies a background color palette to be used for the application.


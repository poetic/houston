Drupal 8 Conversion - Template Data Attribute
=============================================

#Summary of the workflow
	- Getting zip file from Designers
	- Run zip file through Clutch CLI to generate theme
	- Put theme in Drupal site directory and enable it
	- Import all components/content types
	- Create Pages and reference Components

The reason why this process work is because of the `data-attribute`:

	- Clutch CLI looks at the data attributes to generate template
	- Clutch module looks at the data attributes to import Component type and Content type
	- Clutch module looks at the data attributes to find and replace static value with dynamic value.

# How to add data attribute for Drupal 
  Developers can help with this since they are familiar with the following data attributes.

This template is coming from webflow:
			
    <div class="w-section sectionmission">
    	<div class="w-container">
    		<div class="missiondiv">
    			<h3 class="h3 white">OUR&nbsp;MISSION</h3>
    			<h2 class="h2 white italics">To provide emergency shelter, counseling and life changing services to at risk and homeless youth.</h2>
    		</div>
    	</div>
    </div>

After adding data attribute, it should become:

    <div class="w-section sectionmission" data-component="section_mission">
    	<div class="w-container">
    		<div class="missiondiv">
    			<h3 class="h3 white" data-field="title" data-type="string" data-form-type="string_textfield" data-format-type="string">OUR&nbsp;MISSION</h3>
    			<h2 class="h2 white italics"  data-field="description" data-type="string" data-form-type="string_textfield" data-format-type="string">To provide emergency shelter, counseling and life changing services to at risk and homeless youth.</h2>
    		</div>
    	</div>
    </div>

## data-component:
- Clutch CLI uses this to break html file into components.
- Clutch module uses this to create component types.

## data-field: 
- This is part of the field name. We will prefix data-component with data-field to prevent conflict. In this case sectionmission_title and sectionmission_description

## data-type:
- This is the field type.

## data-form-type: 
- This is how the field is viewed in the backend where we edit component/content.

## data-format-type:
- This is how the field is rendered on the front end.

## data-node:
- Clutch CLI uses this to break html file into content types.
- Clutch module uses this to create content types.

These are the most common Field type that Drupal Developers use. You can add more if need.

| Field Type                                          | Form Display                | Display                      |
| ----------------------------------------------------|-----------------------------|------------------------------|
|	string_long (Text - formatted, long, with summary)	|  text_textarea_with_summary |  text_default                |
|	boolean	                                            |  boolean_checkbox					  |  boolean                     |
|	datetime (Date) 	                                  |  datetime_default					  |  datetime_default            |
|	decimal (Number - decimal)						              |  number										  |  number_decimal	             |
|	email																	              |  email_default							|  basic_string		             |
|	integer (Number - integer)						              |  number										  |  number_integer	             |
|	link																                |  link_default							  |  Link                        |
|	list_integer (List - integer)					              |  options_select						  |  list_defaul                 |
|	list_string (List - text)							              |  options_select						  |  list_defaul                 |
|	string_long (Text - plain, long)			              |  string_textarea					  |  basic_string                |
|	string (Text - plain)									              |  string_textfield					  |  string                      |
|	image     														              |  image_image					      |  image/responsive_image      |

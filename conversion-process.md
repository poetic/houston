Drupal 8 Site Conversion
========================

1. Make sure your valkyrie instance is running.
	- You can check it by using `vagrant status`.
	- Also `drush @v uli` to double check that apache2 is running. If it's whitescreen then you have 2 options.
		- drush @v provision-verify
		- vagrant ssh => sudo service apache2 restart

2. Need to update Drush in the vagrant box. 
	- vagrant ssh
	- follow this link to install drush: http://docs.drush.org/en/master/install/. I will also write below the same commands:
			
			# Download latest stable release using the code below or browse to github.com/drush-ops/drush/releases.
			wget http://files.drush.org/drush.phar
			# Or use our upcoming release: wget http://files.drush.org/drush-unstable.phar  

			# Test your install.
			php drush.phar core-status

			# Rename to `drush` instead of `php drush.phar`. Destination can be anywhere on $PATH. 
			chmod +x drush.phar
			sudo mv drush.phar /usr/local/bin/drush

			# Optional. Enrich the bash startup file with completion and aliases.
			drush init

3. Spin up Drupal 8 platform AKA Houston (https://github.com/poetic/houston)
	- You should follow the instruction on the page since this platform required composer manager so it needs 2 extra steps before you can use
	- Since this platform is still in development and required a lot of update, I recommend using the development branch. Assume that you name your valkyrie instance `valkyrie`, you need to `cd valkyrie/platform` then run the following command:

		drush make https://raw.githubusercontent.com/poetic/houston/development/makefiles/stubs/dev.make.yml [platform_name]

	- This will also include a .git so you can contribute if you find any good module that we need.

	- This profile/platform also references another module that we need for Drupal 8 Conversion: Clutch (https://github.com/poetic/houston). The development branch of Houston should pull down the working copy `features/refactor-find-and-replace` branch of the Clutch module (should work with components and content types). 

	**NOTE: AT THIS POINT YOU MIGHT OR MIGHT NOT HAVE AN ISSUE WITH THE DRUSH MAKE. IF YOU RUN THE DRUSH MAKE COMMAND ABOVE AND YOU HAVE THIS ERROR:**

			Beginning to build https://raw.githubusercontent.com/poetic/houston/development/makefiles/stubs/dev.make.yml.
			drupal-8.0.5 downloaded.
			houston cloned from git@github.com:poetic/houston.git.
			Checked out branch development.
			Found makefile: houston.make
			No release history was found for the requested project (clutch).
			
	- If you have the issue above, follow these step:
		- `ls`: just to make sure you're still at the platform level and you can see the new platform you create.
		- `rm -rf [platform_name]`
		- `vagrant ssh => sudo -s => su aegir`
		- `cd /var/aegir/platforms`
		- `drush make https://raw.githubusercontent.com/poetic/houston/development/makefiles/stubs/dev.make.yml [platform_name]`

	- You should see the output like this:

			Beginning to build                                                             [ok]
			https://raw.githubusercontent.com/poetic/houston/development/makefiles/stubs/dev.make.yml.
			drupal-8.0.5 downloaded.                                                       [ok]
			houston cloned from git@github.com:poetic/houston.git.                         [ok]
			Checked out branch development.                                                [ok]
			Found makefile: houston.make                                                   [ok]
			Project admin_toolbar contains 2 modules: admin_toolbar_tools, admin_toolbar.
			admin_toolbar-8.x-1.14 downloaded.                                             [ok]
			composer_manager-8.x-1.0-rc1 downloaded.                                       [ok]
			clutch cloned from git@github.com:poetic/clutch.git.                           [ok]
			Checked out branch features/refactor-find-and-replace.                         [ok]
			coffee-8.x-1.0-beta1 downloaded.                                               [ok]
			Project ctools contains 3 modules: ctools_views, ctools_block, ctools.
			ctools-8.x-3.x-dev downloaded.                                                 [ok]
			devel cloned from http://git.drupal.org/project/devel.git.                     [ok]
			Checked out revision c0b5c5f.                                                  [ok]
			entity_reference_revisions-8.x-1.0-rc4 downloaded.                             [ok]
			Project facets contains 2 modules: core_search_facets, facets.
			facets-8.x-1.0-alpha1 downloaded.                                              [ok]
			libraries-8.x-3.x-dev downloaded.                                              [ok]
			linkit-8.x-4.1 downloaded.                                                     [ok]
			Project metatag contains 2 modules: metatag_open_graph, metatag.
			metatag-8.x-1.0-beta4 downloaded.                                              [ok]
			Project migrate_plus contains 5 modules: migrate_example_setup, migrate_example, migrate_example_advanced_setup, migrate_example_advanced, migrate_plus.
			migrate_plus-8.x-1.0-beta1 downloaded.                                         [ok]
			pathauto-8.x-1.0-alpha2 downloaded.                                            [ok]
			Project paragraphs contains 3 modules: paragraphs_type_permissions, paragraphs_demo, paragraphs.
			paragraphs-8.x-1.0-rc4 downloaded.                                             [ok]
			s3fs-8.x-2.x-dev downloaded.                                                   [ok]
			s3fs patched with s3fs-use_drush_translation_function-2679887-4-8.0.0.patch.   [ok]
			Generated PATCHES.txt file for s3fs                                            [ok]
			Project search_api contains 3 modules: search_api_db_defaults, search_api_db, search_api.
			search_api-8.x-1.0-alpha12 downloaded.                                         [ok]
			token-8.x-1.0-alpha2 downloaded.                                               [ok]

	- The final step is to verify the platform. Go to valkyrie.local. Add platform -> put in the name of your platform and save.

	NOTE: REMEMBER TO RUN THE COMPOSER COMMAND.

4. Get Clutch CLI (https://github.com/cubeddu/clutch)
	- This Clutch CLI will help you generate a theme for Drupal 8 from webflow zip coming from the designer. I also copy down the instruction:
			curl https://cubeddu.github.io/clutch/Download/clutch.phar -L -o clutch.phar

			# Accessing from anywhere on your system:
			mv clutch.phar /usr/local/bin/clutch

			# Apply executable permissions on the downloaded file:
			chmod +x /usr/local/bin/clutch

			# Show all available commands.
			clutch list
	- Assume that you already have a zip file from webflow, run `clutch create:theme` following the instruction, you should see the output similar to this:

			➜  Desktop clutch create:theme
			Please enter the name of the zip file: [webflow] tester
			Please enter theme name: [webflow] poetic
			Please enter theme description: [These is a webflow theme]
			Starting Theme creation process
			poetic/components/header/header.html.twig
			poetic/components/section/section.html.twig
			poetic/components/sectionmission/sectionmission.html.twig
			poetic/components/sectioncards/sectioncards.html.twig
			poetic/components/sectionsupplies/sectionsupplies.html.twig
			poetic/components/background/background.html.twig
			Good Job your theme is ready!

5. Spin up new d8 site. 
	- Go to valkyrie.local
	- Add new site like you usually add, for example `clutch.com`
	- Select the correct platform and Houston profile
	- Save and you should have a d8 site in 5'

6. Start the conversion process for Component
	- copy the theme you just generated earlier to your site theme directory: clutch.com/themes
	- drush use @clutch-com if you want to target the site 
	- drush uli to login to the site
	- Go to appearance and enable your new theme
	- Go to Configuration, you should see Clutch API Form. Go to Clutch API Form
	- On the next screen, you should see a list of new components that you can import. Select all and run Create

7. Create page and associate the component (remember to read this first https://github.com/poetic/clutch/tree/features/readme)
	- Make sure you update the Url Pattern for Custom Page
	- Structure => Page List => Add new page
	- Select all the components you want to render on the page then save

8. Start the conversion process for Node
	- Go to Configuration, you should see Clutch Node API Form. Go to Clutch Node API Form
	- Select all content types run Create

	**NOTE:** the difference between Node and Component is the Clutch API Form will actually create the actual content for each type while the Clutch Node API Form will only create the structure of the content type.

## IMPORTANT: THE CLUTCH CLI AND THE FUNCTIONALITY OF THE CLUTCH MODULE RELY HEAVILY ON DATA ATTRIBUTE IN THE TEMPLATE. I WILL ALSO WRITE ANOTHER DOCUMENT ON HOW DESIGNERS/DEVELOPERS CAN HELP WITH THIS.(Please read https://github.com/poetic/houston/blob/development/template-instruction.md)

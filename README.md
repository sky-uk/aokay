[![Build Status](https://travis-ci.org/sky-uk/aokay.svg?branch=master)](https://travis-ci.org/sky-uk/aokay)
[![Coverage Status](https://coveralls.io/repos/sky-uk/aokay/badge.svg?branch=master)](https://coveralls.io/r/sky-uk/aokay?branch=master)

aokay
=====

Helps you to write tests which verify Omniture requests are made correctly.

Installation
------------

Add this line to your application's Gemfile:

    gem 'aokay', :git => 'git@github.com:sky-uk/aokay.git', :tag => 'vX.X.X'

*Where tag is the latest release*

And then execute:

    bundle

Usage
-----

aokay allows you to make assertions on which variables or events are being sent to Omniture. 

### RSpec

#### In spec helper file:

    require 'aokay/rspec'
    RSpec.configure do |config|

      config.aokay_sitecat_refs =
        {
          :ab_group => 'v18',
          :section => 'c27',
          :contentType => 'c20',
          :pageName => 'pageName',
          :event => 'events',
          :url => { prop: 'url', eVar: 'url' }
        }
      config.aokay_sitecat_url = 'mysitecaturl.com'
    end

### Cucumber

In `features/support/env.rb`, require the gem and setup the field_refs

    require 'aokay/cucumber'

    Aokay::Configuration.field_ref = { 
      :ab_group => 'v18',
      :section => 'c27',
      :contentType => 'c20',
      :pageName => 'pageName',
      :event => 'events',
      :url => { prop: 'url', eVar: 'url' }
    }
    Aokay::Configuration.sitecat_url = 'mysitecaturl.com'

### Minitest

In your `test_helper.rb`, require the gem and setup the field_refs

    require 'aokay/minitest'

    Aokay::Configuration.field_ref = {
      :ab_group => 'v18',
      :section => 'c27',
      :contentType => 'c20',
      :pageName => 'pageName',
      :event => 'events',
      :url => { prop: 'url', eVar: 'url' }
    }
    Aokay::Configuration.sitecat_url = 'mysitecaturl.com'

### Within your specs

To check page tracking in Adobe SiteCatalyst:

`expect(last_sitecat_request[:page (, :prop/eVar)]).to end_with "/requests/new"`

To check the custom variable 'ab_group', which is set to eVar18 in our omniture
config. Use the following: 

`expect(last_sitecat_request[:ab_group (, :prop/eVar)]).to eq "testGroupAlpha"`

The same can also be used for your Google Analytics tests

`expect(last_google_analytics_request[:url (, :prop/eVar)]).to end_with(this_page)`

Or with minitest

`last_sitecat_request.tracked(:siteName (, :prop/eVar)).must_equal 'yourSiteName'`

*Code in brackets is depending on configuration. `:event => 'events' or :url => { prop: 'url', eVar: 'url' }` etc.*

Dependencies
------------

You need phantomjs. Please check with your package manager.

Contributing
------------

1. Fork it ( `http://github.com/<my-github-username>/aokay/fork` )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

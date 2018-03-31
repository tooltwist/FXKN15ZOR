# vue-authservice
VueJS components for authservice.io






# Installation

    npm install vue-authservice debounce --save

or

    yarn add vue-authservice debounce
    

#### Adding to a VueJS project

When used with a module system, you must explicitly install Vuex via Vue.use():

    import Vue from 'vue'
    import Authservice from 'vue-authservice'
    
    Vue.use(Authservice, options)


#### Adding to a Nuxt project

Authservice is added to a Nuxt project by creating a Nuxt plugin.

~/plugins/vue-authservice:

    import Vue from 'vue'
    import Authservice from 'vue-authservice'

    // Load the configuration. This directory should be included in .gitignore.
    import Config from '../protected-config/websiteConfig'

    const options = ...  // See below
    Vue.use(Authservice, options)

nuxt.config.js:

    module.exports = {
      ...
      plugins: [
        ...
        { src: '~plugins/vue-authservice.js', ssr: false },
      ],
    }
    
## Your Account Dashboard
To use Authservice you will need to create a free account at http://tooltwist.com and
get the API for your application.


## Options

vue-authservice requires that an `options` object is passed to Vue.use().

### Mandatory Options
These options relate to how your client application connects to the remote Authservice.io server.

Some of these values may change during the different stages of your development, so the endpoint
details are best saved in a configuration file, that can be overwritten during deployment. The
convention we use is to place such a file in a directory named `protected-config/authservice-config.js`.

protected-config/websiteConfig.js:

    /*
     *  This file gets overwritten during production deployments.
     */
    module.exports = {
      authservice: {
        host: 'authservice.io',
        version: 'v2',
        apikey: 'API10O0X1NS8FWUTO3FXKN15ZOR09'
      }
    }

We then reference this file when setting our endpoints. Note that not all the values need to be defined.


    // Load the configuration. This directory should be included in .gitignore.
    import Config from '../protected-config/websiteConfig'

    const options = {
      protocol: Config.authservice.protocol,
      host: Config.authservice.host,
      port: Config.authservice.port,
      version: Config.authservice.version,
      apikey: Config.authservice.apikey,
      hints: {
        sitename: 'ToolTwist',
      }
      ...
    }

Most of these endpoint values are provided when you get the APIKEY from the ToolTwist website.


Option | Default | Notes
------ | ------- | -----------
protocol | https              | http or https
host     | api.authservice.io | Enterprise customers have dedicated servers
port     | 80                 |
version  | v2                 |
apikey   | mandatory          | Allocate APIKEYs with your tooltwist.com account
sitename | 'this site'        | Name of your website / company, used in prompts


### Registration

Allowing users to sign up using their email address is optional. To disable
email registration, set `register` to `false`.

    const options = {
      ...
      hints: {
        register: false,
        ...
      }
    }

If you _do_ want to allow user self-registration, provide the options like this:

    const options = {
      ...
      hints: {
        register: {
          password: true,
          firstname: false,
          middlename: false,
          lastname: false,
          resumeURL: 'http://mydomain.com/welcome',
          termsMessage: 'Agree to our terms?',
          termsRoute: '/terms-and-conditions'
        },
        login : {
          registerMessage: 'Don\'t have an account yet?'
        },
        ...
      }
    }

For most applications it is desirable to keep the registration process as simple as possible
    
    
Option | Default | Notes
------ | ------- | -----------
password | true              | If `false` the user will not be prompted for a password.
firstname | false | Prompt the user for their first name
middlename | false | Prompt the user for their middle name
lastname | false | Prompt the user for their last name
resumeURL | mandatory | Where the useer is sent after clicking the link in the email they are sent
termsMessage | By signing up to <sitename> you agree to our EULA | Message on the bottom of the sign up page
termsRoute | /terms-and-conditions | URL of your EULA page
registerMessage | 'New to <sitename>?' | Sign in message shown on the login page
    
### Forgotten password

The optional 'forgotten password' option allows an email to be sent to the user, containing a
link to a 'reset password' page on your site. You will need to provide this page, and provide
it's URL as `resumeURL`.


    const options = {
      ...
      hints: {
        forgot: {
          resumeURL: 'http://mydomain.com/password-reset'
        }
      }
    }

To disable forgotten password functionality, set `forgot` to `false`.

    const options = {
      ...
      hints: {
        register: false,
        ...
      }
    }

If you _do_ want to allow user self-registration, provide the options like this:

    const options = {
      ...
      hints: {
        register: {
          password: true,
          firstname: false,
          middlename: false,
          lastname: false,
          resumeURL: 'http://mydomain.com/welcome',
          termsMessage: 'Agree to our terms?',
          termsRoute: '/terms-and-conditions'
        },
        login : {
          registerMessage: 'Don\'t have an account yet?'
        },
        ...
      }
    }


## Overriding defaultLogin options
The options for a user logging in are downloaded from the Authservice server, and are controlled
by the Dashboard for your account at tooltwist.com.

The options below can be used to disable this login options.

For example, you may have Facebook login configured on the Admin dashboard, but do not want to
allow it from this application.

However, if you do not have Facebook login configured in the Admin dashboard, an error will occur if
you try to enable it here.

    const options = {
      ...

      hints: {
        usernames: true,
        login: {
          email: false,
          facebook: true,
          github: true,
          google: true,
          linkedin: true,
          twitter: true,
        },
      }
    }

Option | Default | Notes
------ | ------- | -----------
usernames | false | Are users required to have a unique username
email | true | If disabled, the user will be forced to use a social media login
facebook | false | Allow Facebook login
github | false | Allow Github login
google | false | Allow Google login
linkedin | false | Allow Linkedin login
twitter | false | Allow Twitter login

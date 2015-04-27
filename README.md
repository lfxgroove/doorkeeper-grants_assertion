# Doorkeeper - Assertion Grant Extension

Assertion grant extension for Doorkeeper. Born from:
https://github.com/doorkeeper-gem/doorkeeper/pull/249

## Installation

1. Add both gems to your `Gemfile`.
2. Add `assertion` as a `grant_flow` to your initializer.

___

Lets you define your own way of authenticating resource owners via 3rd Party
applications. For example, via Facebook:

```ruby
Doorkeeper.configure do
  resource_owner_from_assertion do
    facebook = URI.parse('https://graph.facebook.com/me?access_token=' +
    params[:assertion])
    response = Net::HTTP.get_response(facebook)
    user_data = JSON.parse(response.body)
    User.find_by_facebook_id(user_data['id'])
  end
  
  # add your supported grant types and other extensions
  grant_flows %w(assertion authorization_code implicit password client_credentials)
end
```

To access this endpoint you would then make a request that looks like the following:
```
/oauth/token?grant_type=assertion&assertion=...
```
Where you replace `...` with your assertion, be it a jwt or something else. 

You will probably also want to create a route in your auth server that can create
assertions for you.

___

IETF standard: http://tools.ietf.org/html/draft-ietf-oauth-assertions-16

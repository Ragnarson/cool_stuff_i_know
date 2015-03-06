# Using multiple models in model hook in routes

``` javascript
import Ember from 'ember';

export default Ember.Route.extend({
  model: function(params) {
    return Ember.RSVP.hash({
      user: // somehow get the user
      post: // somehow get the post
      comments: // // somehow get the comments
    });
  },
});
```
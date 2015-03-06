# Sorting collections outside ArrayController

``` javascript
import Ember from 'ember';

export default Ember.Controller.extend({
  comments: (function() {
    return Ember.ArrayProxy.createWithMixins(Ember.SortableMixin, {
      sortProperties: ['published_at'],
      sortAscending: true,
      content: this.get('model.comments')
    });
  }).property('model.comments.@each'),
});
```
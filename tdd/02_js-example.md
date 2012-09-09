!SLIDE
# 2. Better design

.notes Testing code that hasn't been written tests beforehand is a nightmare. (example jquery code)
.notes You write your code very differently as you're writing it. (use bkeeper's example)
.notes Solving design problems solves testing problems

!SLIDE
# Testing is hard when we write bad code

!SLIDE
<iframe src="http://www.matthew-steele.com/talks/barcamp-app/index.html" width="100%" height="600"></iframe>

!SLIDE smaller

    @@@ javascript
    jQuery(function($) {
        $('form').on('submit', function() {
          $.ajax({
            url: '/statuses',
            type: 'POST',
            dataType: 'json',
            data: {text: $(this).find('textarea').val()},
            success: function(data) {
              $('#statuses').append('<li>' + data.text + '</li>');
            }
          });
        return false;
        });

!SLIDE smaller

    @@@ javascript
        $.ajax({
          url: '/statuses',
          dataType: 'json',
          success: function(data) {
            var $statuses = $('#statuses');
            for(var i = 0; data.length > i; i++) {
              $statuses.append('<li>' + data[i].text + '</li>');
            }
          }
      })
    });

!SLIDE smaller
    @@@ javascript
    describe("statuses", function() {
      var statuses;
      beforeEach(function() {
      statuses = new Monologue.Collection.Statuses();
      });

      it('has a Status as its model', function() {
        expect(statuses.model).toEqual(Monologue.Model.Status);
      });

      it('has its root URL as /statuses', function() {
        expect(statuses.url).toEqual('/statuses');
      });
    });

!SLIDE smaller
    @@@ javascript
    var Monologue = {
      Model: {},
      View: {},
      Collection: {}
    };

!SLIDE smaller

    @@@ javascript
    Monologue.Model.Status = Backbone.Model.extend({
    });

    Monologue.Collection.Statuses = Backbone.Collection.extend({
      model: Monologue.Model.Status,
      url: '/statuses'
    });


!SLIDE
<iframe src="http://www.matthew-steele.com/talks/barcamp-app/specs/index.html" width="100%" height="600"></iframe>

!SLIDE smaller

    @@@javascript
    describe('Post Status Form', function() {
      var view, $el, collection;

      beforeEach(function() {
        $el = $("<form><textarea>Whee!</textarea></form>");
        collection = new (Backbone.Collection.extend({
          url: '/mock'
        }));
        spyOn(collection, 'create');

        view = new Monologue.View.PostStatus({
          el: $el, 
          collection: collection
        });
      });

!SLIDE smaller
    @@@javascript
    describe("submitting the form", function() {
      it("creates status when submitting the form", function() {
        $el.trigger('submit');
        expect(collection.create)
          .toHaveBeenCalledWith({text: "Whee!"});
        });
      });
    });


!SLIDE smaller
    @@@javascript
    Monologue.View.PostStatus = Backbone.View.extend({
      events: {
        'submit' : 'submit'
      },

      submit: function(e) {
        e.preventDefault();
        var $input = this.$el.find('textarea');
        this.collection.create({text: $input.val()});
        return false;
      }
    });

!SLIDE
<iframe src="http://www.matthew-steele.com/talks/barcamp-app/specs/index.html" width="100%" height="600"></iframe>


!SLIDE smaller
    @@@javascript
    it("clears the form", function() {
      $el.trigger('submit');
      expect($el.find('textarea').val()).toEqual('');
    });

!SLIDE smaller
    @@@javascript
    submit: function(e) {
      e.preventDefault();
      var $input = this.$el.find('textarea');
      this.collection.create({text: $input.val()});
      $input.val('');
      return false;
    }

!SLIDE
<iframe src="http://www.matthew-steele.com/talks/barcamp-app/specs/index.html" width="100%" height="600"></iframe>

!SLIDE smaller
    @@@javascript
    describe("Status List", function() {
      var view, $el, collection;

      beforeEach(function() {
        $el = $('<div />');
        collection = new (Backbone.Collection.extend({
          url: '/mock'
        }));
        spyOn(collection, 'fetch');

        view = new Monologue.View.StatusList({
          el: $el,
          collection: collection
        });
      });

      it('fetches records from the server', function() {
        expect(collection.fetch).toHaveBeenCalled();
      });

!SLIDE smaller
    @@@javascript
    Monologue.View.StatusList = Backbone.View.extend({
      initialize: function() {
        this.collection.fetch();
      }
    });

!SLIDE smaller
    @@@javascript
    it('renders when collection is reset', function() {
      collection.reset([{text: 'Reset text'}]);
      var $li = $el.find('li');
      expect($li.length).toBe(1);
      expect($li.text()).toEqual('Reset text');
    });

!SLIDE smaller
    @@@javascript
    Monologue.View.StatusList = Backbone.View.extend({
      initialize: function() {
        _.bindAll(this, 'render', 'add');
        this.collection.on('reset', this.render);
        this.collection.on('add', this.add);
        this.collection.fetch();
      },

      render: function() {
        this.collection.each(_.bind(this.add, this));
      },

      add: function(model) {
        this.$el.append(this.template(model));
      },

      template: function(model) {
        return this.make('li', 
                        {'class':'status'}, 
                        model.get('text'));
      }

!SLIDE
<iframe src="http://www.matthew-steele.com/talks/barcamp-app/specs/index.html" width="100%" height="600"></iframe>

!SLIDE smaller
    @@@javascript
    jQuery(function($) {
      var statuses = new Monologue.Collection.Statuses();

      new Monologue.View.PostStatus({
        el: $('#new-status'), 
        collection: statuses
      });

      new Monologue.View.StatusList({
        el: $('#statuses'),
        collection: statuses
      });
    });

!SLIDE
<iframe src="http://www.matthew-steele.com/talks/barcamp-app/specs/index.html" width="100%" height="600"></iframe>


!SLIDE smaller
    @@@javascript
    describe('local storage', function() {
      it('uses localStorage', function() {
        expect(statuses.localStorage).toBeDefined();
      });

      it('sync to a Backbone localStorage', function() {
        var ls = statuses.localStorage;
        expect(ls.name).toEqual('Statuses');
      });
    });

!SLIDE smaller
    @@@javascript
    Monologue.Collection.Statuses = Backbone.Collection.extend({

      model: Monologue.Model.Status,
      localStorage: new Backbone.LocalStorage('Statuses')

    });

!SLIDE
<iframe src="http://www.matthew-steele.com/talks/barcamp-app/specs/index.html" width="100%" height="600"></iframe>

!SLIDE smaller
    @@@javascript
    jQuery(function($) {
      $('form').on('submit', function() {
        $.ajax({
          url: '/statuses',
        type: 'POST',
        dataType: 'json',
        data: {text: $(this).find('textarea').val()},
        success: function(data) {
          $('#statuses').append('<li>' + data.text + '</li>');
        }
        });
        return false;
      });

!SLIDE smaller
    @@@javascript
      $.ajax({
        url: '/statuses',
        dataType: 'json',
        success: function(data) {
          var $statuses = $('#statuses');
          for(var i = 0; data.length > i; i++) {
            $statuses.append('<li>' + data[i].text + '</li>');
          }
        }
      })
    });

!SLIDE smaller
    @@@javascript
    var Monologue = {
      Model: {},
      View: {},
      Collection: {}
    };

    Monologue.Model.Status = Backbone.Model.extend({
    });

    Monologue.Collection.Statuses = Backbone.Collection.extend({
      model: Monologue.Model.Status,
      localStorage: new Backbone.LocalStorage('Statuses')
    });

!SLIDE smaller
    @@@javascript
    Monologue.View.PostStatus = Backbone.View.extend({
      events: {
        'submit' : 'submit'
      },
      submit: function(e) {
        e.preventDefault();
        var $input = this.$el.find('textarea');
        this.collection.create({text: $input.val()});
        $input.val('');
        return false;
      }
    });

!SLIDE smaller
    @@@javascript
    Monologue.View.StatusList = Backbone.View.extend({
      initialize: function() {
        _.bindAll(this, 'render', 'add');
        this.collection.on('reset', this.render);
        this.collection.on('add', this.add);
        this.collection.fetch();
      },

      render: function() {
        this.collection.each(_.bind(this.add, this));
      },

      add: function(model) {
        this.$el.append(this.template(model));
      },

      template: function(model) {
        return this.make('li', {'class':'status'}, model.get('text'));
      }
    });

!SLIDE smaller
    @@@javascript
    jQuery(function($) {
      var statuses = new Monologue.Collection.Statuses();

      new Monologue.View.PostStatus({el: $('#new-status'), collection: statuses});
      new Monologue.View.StatusList({el: $('#statuses'), collection: statuses});
    });


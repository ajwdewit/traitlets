# Traitlets_pcse

This is a fork of [ipython/traitlets](https://github.com/ipython/traitlets) with the sole difference of 
allowing notifications on traitlets of `type=All`. So given the piece of code below:

```python

    from traitlets_pcse import HasTraits, Float, All
    
    class TestNotification(HasTraits):
        a = Float()
    
        def __init__(self, *args, **kwargs):
            super().__init__(*args, **kwargs)
            self.observe(self._notify_change1, names="a", type='change')
            self.observe(self._notify_change2, names="a", type=All)
    
        def _notify_change1(self, change):
            s = "This notifier will only be triggered by a change in value: attribute '{name}', old_value: {old}, new_value: {new} of type '{type}'"
            print(s.format(**change))
    
        def _notify_change2(self, change):
            s = "This notifier will always be triggered: attribute {name}, old_value: {old}, new_value: {new} of type '{type}'"
            print(s.format(**change))
    
    t = TestNotification(a=1)
    print("#############")
    t.a = 2
    print("#############")
    t.a = 2
```   

Will output:

```
    #############
    This notifier will only be triggered by a change in value: attribute 'a', old_value: 1.0, new_value: 2.0 of type 'change'
    This notifier will always be triggered: attribute a, old_value: 1.0, new_value: 2.0 of type 'traitlets.All'
    #############
    This notifier will always be triggered: attribute a, old_value: 2.0, new_value: 2.0 of type 'traitlets.All'
```   

I only created this package because the default traitlets package does not support it (yet) and I need to be
able to specify it as a dependency without interfering with the default traitlets. The latter will come when installing
IPython. Only use this package if you need such functionality, otherwise go the standard
traitlets package.

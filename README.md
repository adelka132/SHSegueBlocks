SHSegueBlock
==========

Overview
--------

SHSegueBlock is a category on top of UIViewController to allow block based segueus without the bullshit of swizzling and other disgusting hacks - also adds the ability to set userInfo on top of a viewController. It's built on top of NSMapTable that works with weakToWeak references between a controller and its blocks and userInfo. 


Swizzle & Junk free 
-------------------

No developer psyches were harmed or killed for this. I've noticed other similar libraries all swizzle like there is no tomorrow. If the API can remains the same without Swizzle, then don't fucking Swizzle. 

All in all; 100 loc for both userInfo as well as block based segueus.
The blocks are gone as soon the segue has finished and userInfo content are gone as soon as the controllers are gone or you set it to nil. 


Installation
------------

```ruby
pod 'SHSegueBlock'
```

***

Setup
-----

Put this either in specific controllers or your project prefix file

```objective-c
#import 'SHSegueBlock.h'
```


Usage
-----

With SHSegueBlock you do it all in one place, like so:

```objective-c
  [self SH_performSegueWithIdentifier:@"push" 
        andDestionationViewController:^(UIViewController * theDestinationViewController) {

    theDestinationViewController.whateverPropety = anotherLocalVariable

  }];

``` 

or if you want access to the full segue object

```objective-c
  [self SH_performSegueWithIdentifier:@"push" 
              andPrepareForSegueBlock:^(UIStoryboardSegue *theSegue) {

    id<SHExampleProtocol> destionationController =   theSegue.destinationViewController;
    destionationController.name = theSegue.identifier;

  }];

```


Bonus - SH_userInfo property
------ 

You can directly set a userInfo (mutable) dictionary directly on the segueu selector for the destination controller

```objective-c
[self SH_performSegueWithIdentifier:@"unwinder" withUserInfo:@{@"date" : [NSDate date]}];

```

In the destinationViewController

```objective-c
self.myDate = self.SH_userInfo[@"date"];
```

or

```objective-c
  [self SH_performSegueWithIdentifier:@"push" 
        andDestionationViewController:^(UIViewController * theDestinationViewController) {

    theDestinationViewController.SH_userInfo = myDictionary

  }];

``` 

Existing Codebase 
-----------------

If you already have  

```objective-c
-(void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender;
``` 

implemented and used within your code base you can use the block handler

```objective-c
-(BOOL)SH_handlesBlockForSegue:(UIStoryboardSegue *)theSegue;
```

Like this 

```objective-c
-(void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender; {
  UIViewController * destionationVc = segue.destinationViewController;
  destionationVc.SH_userInfo = nil;
  if([self SH_handlesBlockForSegue:segue])
    NSLog(@"Performed segueue programatically user info: %@", destionationVc.SH_userInfo);
  else
    NSLog(@"Performed unwind segueue via IB");
}

```
That will check if there is block **and** if there is - execute it. 

Replacing
---------

```objective-c
[self performSegueWithIdentifier:@"theIdentifier" sender:nil];
```

and then implementing the callback (notice how you need a property to pass stuff over)

```objective-c
-(void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender; {
  UIViewController * destinationViewController = segue.destinationViewController;
  destionationViewController.whateverPropety = self.anotherProperty
}
```


Contact
-------

If you end up using SHSegueBlock in a project, I'd love to hear about it.

email: [seivan.heidari@icloud.com](mailto:seivan.heidari@icloud.com)  
twitter: [@seivanheidari](https://twitter.com/seivanheidari)

## License

SHSegueBlock is © 2013 [Seivan](http://www.github.com/seivan) and may be freely
distributed under the [MIT license](http://opensource.org/licenses/MIT).
See the [`LICENSE.md`](https://github.com/seivan/SHSegueBlock/blob/master/LICENSE.md) file.
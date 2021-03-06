# LGFlapJackStackView

[![Version](https://img.shields.io/cocoapods/v/LGFlapJackStackView.svg?style=flat)](http://cocoapods.org/pods/LGFlapJackStackView)
[![License](https://img.shields.io/cocoapods/l/LGFlapJackStackView.svg?style=flat)](http://cocoapods.org/pods/LGFlapJackStackView)
[![Platform](https://img.shields.io/cocoapods/p/LGFlapJackStackView.svg?style=flat)](http://cocoapods.org/pods/LGFlapJackStackView)

<p align="center">
  <img src="https://raw.githubusercontent.com/lukegeiger/LGFlapJackStackView/master/lukegeiger-flapjackstackview-ios-objective-c-detroit.png">
</p>

## Motivation
When creating my app HoopMetrics, I found myself in need of a type of graph that would display the result of two numbers competing with one another in a specific category. The greater the difference in their competition, the more dominate it needed to appear visually. This is the graph that was my solution to that problem. If you need to display a head-to-head matchup, this graph may be useful for you.

## Usage
There are three classes that play together to make this graph happen. A LGFlapJack, a LGFlapJackCell, and a LGFlapJackStackView. All you need to do is create LGFlapJack model items, store them in array, and pass them to the LGFlapJackView, and the rest is handled for you. This graph is exportable via CSV and Image. (See Exportability)

```objective-c

    //For the purposes of this demo
    NSMutableArray*flapJacks = [NSMutableArray new];
    for (int i = 0; i<12; i++) {
        LGFlapJack *flapJack = [LGFlapJack new];
        flapJack.leftBarTotal = [NSNumber numberWithInt:[self getRandomNumberBetween:0 to:100]];
        flapJack.rightBarTotal = [NSNumber numberWithInt:[self getRandomNumberBetween:0 to:100]];
        //Each bar can be its own color.
        flapJack.leftBarColor = [UIColor colorWithRed:17/255. green:159/255. blue:194/255. alpha:1.0];
        flapJack.rightBarColor = [UIColor colorWithRed:206/255. green:218/255. blue:60/255. alpha:1.0];
        flapJack.inlineString = [self randomCategoryName];
        [flapJacks addObject:flapJack];
    }
    
    self.lgFlapJackStackView = [[LGFlapJackStackView alloc]initWithFrame:self.view.bounds];
    self.lgFlapJackStackView.flapJacks = flapJacks;
    self.lgFlapJackStackView.flapJackHeight = 42;
    self.lgFlapJackStackView.barLabelFont = [UIFont fontWithName:@"HelveticaNeue-Light" size:12];
    self.lgFlapJackStackView.barLabelTextColor = [UIColor colorWithRed:85/255. green:85/255. blue:85/255. alpha:1.0];
    self.lgFlapJackStackView.inlineLabelFont =  [UIFont fontWithName:@"HelveticaNeue-Light" size:12];
    self.lgFlapJackStackView.inlineLabelTextColor = [UIColor colorWithRed:100/255. green:100/255. blue:100/255. alpha:1.0];
    self.lgFlapJackStackView.tableFooterView = [self sampleFooterView];
    
    //Name of the graph that is shown in email attachements.
    self.lgFlapJackStackView.name = @"lukegeiger";
    
    [self.view addSubview:self.lgFlapJackStackView];

```
## Exportability
Currently, the LGFlapJackStackView supports being exported to a CSV or as an image. You may find these functions useful to achieve that. To customize these further, look into subclassing the LGFlapJackStack view, or submit an issue on GitHub.

```objective-c
//LGFlapJackStackView.h
-(UIImage*)graphAsImage;
-(NSString*)graphAsCSVStringWithColOne:(NSString*)colOne colTwo:(NSString*)colTwo colThree:(NSString*)colThree;
```

As a reminder, to add a CSV string to an email attachment, you would do something like the following.

```objective-c
//An excerpt from the demo. 
MFMailComposeViewController *mailViewController = [MFMailComposeViewController new];
[mailViewController setMessageBody:@"If you enjoyed this demo, follow me on twitter! @lukejgeiger" isHTML:NO];
mailViewController.mailComposeDelegate = self;
            
//Converts NSString to NSData. You then take this data and add it to the MFMailComposeViewController.
NSData*data = [[self.lgFlapJackStackView graphAsCSVStringWithColOne:@"Category" colTwo:@"Blue Team" colThree:@"Green Team"]dataUsingEncoding:NSUTF8StringEncoding];
  
//Adding it the controller. Don't forget this step!
[mailViewController addAttachmentData:data mimeType:@"text/csv" fileName:[NSString stringWithFormat:@"%@.csv",self.lgFlapJackStackView.name]];

[self presentViewController:mailViewController animated:YES completion:nil];
```

A CSV attachment looks something like this. The bar column names are customizble.

| Category  | Blue Team | Green Team |
|-----------|-----------|------------|
| PTS       | 34        | 53         |
| RBD       | 44        | 22         |
| AST       | 10        | 55         |


##Customization
All colors, fonts, etc are editable. If you want to edit the label that is in the bars of the flapjack, simply subclass LGFlapJack, and override rightBarFormatString or leftBarFormatString. 

As it stands right now, there are no default headers or footers for this graph, but you can pass in a custom view if you wish. (See Possible Improvements section)

## Edge Cases
As it sits right now, if one flapjack's right or left number is severely dominating, it pushes out the lesser to a point where you can't see it displayed on the graph. I am looking to add a maximumWidth property on the bars soon.

## Possible Improvements
- Some sort of animation. But the animation should serve some purpose, and it shouldn't just animate just to animate.
- Default Footer/Header.
- Adding/Removing Flap Jacks.
- Sorting Flap Jacks.

## Implementation Notes
- What appears to be rows in this table view, are actually sections. I chose to implement it like this because later down the line I may want to take advantage of the titleForSection or footerForSection methods on the UITableView's data source protocol.

## Installation

LGFlapJackStackView is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod "LGFlapJackStackView"
```

## Author

Luke Geiger, lukejamesgeiger@gmail.com

@lukejgeiger

## License

LGFlapJackStackView is available under the MIT license. See the LICENSE file for more info.

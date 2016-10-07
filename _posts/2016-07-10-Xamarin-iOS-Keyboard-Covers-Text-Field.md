---
layout: post
lang: C#
nav_blog: class="selected"
title: Xamarin iOS Keyboard Covers Text Field
comments: true
description : Scrolling content to the current UI control to avoid keyboard covering the control
date: 2016-10-07
header_desc: Xamarin iOS - Keyboard covers text field
---
<p></p>

{% highlight csharp linenos %}

public partial class Details : UIViewController
{
    private UIView activeview; // stores active view information
        
    public override void ViewDidLoad()
    {
        // Show Keyboard
        UIToolbar kbToolbar = new UIToolbar(RectangleF.Empty);
        kbToolbar.BarStyle = UIBarStyle.Default;
        kbToolbar.Translucent = true;
        kbToolbar.UserInteractionEnabled = true;
        kbToolbar.SizeToFit();
        UIBarButtonItem btnKBFlexibleSpace = new UIBarButtonItem(UIBarButtonSystemItem.FlexibleSpace, null);
        UIBarButtonItem btnKBDone = new UIBarButtonItem(UIBarButtonSystemItem.Done, KBToolbarButtonDoneHandler);
        UIBarButtonItem[] btnKBItems = new UIBarButtonItem[] { btnKBFlexibleSpace, btnKBDone };
        kbToolbar.SetItems(btnKBItems, true);
    
        // Link keyboard to the Text Control
        SampleTextBox.InputAccessoryView = kbToolbar;
        SampleTextBox.ClipsToBounds = true;
        SampleTextBox.LayoutIfNeeded();
    
        // Keyboard popup
        NSNotificationCenter.DefaultCenter.AddObserver
        (UIKeyboard.DidShowNotification, KeyBoardUpNotification);

        // Keyboard Down
        NSNotificationCenter.DefaultCenter.AddObserver
        (UIKeyboard.WillHideNotification, KeyBoardDownNotification);
    }
    
    private void KeyBoardUpNotification(NSNotification notification)
    {
        // get the keyboard size
        CoreGraphics.CGRect r = UIKeyboard.BoundsFromNotification(notification);

        // Find what opened the keyboard
        foreach (UIView view in this.View.Subviews)
        {
            if (view.IsFirstResponder)
                activeview = view;
        }

        if (activeview != null)
        {
            // Bottom of the controller = initial position + height + offset      
            bottom = ((float)(activeview.Frame.Y + activeview.Frame.Height + offset));

            // Calculate how far we need to scroll
            scroll_amount = ((float)(r.Height - (View.Frame.Size.Height - bottom)));

            // Perform the scrolling
            if (scroll_amount > 0)
            {
                moveViewUp = true;
                ScrollTheView(moveViewUp);
            }
            else
            {
                moveViewUp = false;
            }
        }

    }
    private void KeyBoardDownNotification(NSNotification notification)
    {
        if (moveViewUp) { ScrollTheView(false); }
    }
    private void ScrollTheView(bool move)
    {
        // scroll the view up or down
        UIView.BeginAnimations(string.Empty, System.IntPtr.Zero);
        UIView.SetAnimationDuration(0.1);

        CoreGraphics.CGRect frame = View.Frame;

        if (move)
        {
            frame.Y -= scroll_amount;
            xSeg.Hidden = true;
        }
        else
        {
            frame.Y += scroll_amount;
            scroll_amount = 0;
            xSeg.Hidden = false;
        }

        View.Frame = frame;

        UIView.CommitAnimations();
    }
    public void KBToolbarButtonDoneHandler(object sender, EventArgs e)
    {
        SampleTextBox.ResignFirstResponder();
    }
}

{% endhighlight %}

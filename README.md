Cloned from this site
https://www.codeproject.com/Articles/45684/Culture-Aware-Month-Calendar-and-Datepicker

Image 1
Introduction

As you can see in the screenshot, this is a month calendar control and date picker control, a little like the built-in controls from Microsoft. It's not implemented using WPF, but is for WinForms applications.
Background

While developing my own calendar/appointment calendar similar to the one in Outlook 2007, I needed a month calendar. There are many good ones out there, but not quite the way I needed. So, I started to develop my own. Along the way, I implemented not only a month calendar but also a date picker control.

Also, I thought, why not implement culture awareness into the controls, so people in different countries can use it too. That was more complex than I thought, and I ran into many problems regarding culture support, including some nasty bugs in the framework; more in detail later.

I thought to share the results here, because CodeProject has offered me solutions to my problems several times.
Class Diagram

Image 2

There is a new property AllowPromptAsInput in the DatePicker control.
Setting it to true allows to enter dates including the current date separator as set in the
FormatProvider property.
Features

    culture aware - meaning you can set the used culture and calendar
    adjustable number of visible sub-calendars via the property CalendarDimensions
    fully customizable in regard to colors and fonts
    you can load or save the color table
    adjustable RTL or LTR layout
    can adjust whether to use shortest or abbreviated day names
    can adjust whether to show week header and footer
    adjustable non working days (for work week selection mode)
    adjustable first day of week
    adjustable day names (full/abbreviated/shortest)
    adjustable month names (full/abbreviated)
    adjustable month/day string patterns
    supports settable min/max date
    scrolling with mouse wheel
    different selection modes (single day/work week/full week/manual)
    adjustable maximum selectable days in manual selection mode
    bolded days collection
    new: extended bolded days (category and corresponding color definition)
    new: the manual selection mode now supports selection of multiple ranges with the CTRL key
    new: date separator now valid input in picker control
    adjustable scroll change (how many months to scroll when clicking an arrow)
    date picker control supports keyboard navigation
    disabled state support

Using the Code

Simply add the control project to your existing solution and reference it. The control is built with VS 2010, and the targeted framework version is 3.5.
Points of Interest

Regarding the culture awareness of the controls, I must admit that I cannot guarantee that all is correctly displayed and calculated. If there are errors, please feel free to post a message.

As mentioned earlier, while implementing culture awareness, I encountered some nasty bugs in the framework. One of these is as follows:

Every System.Globalization.Calendar implementation has a method GetWeekOfYear(). And, every calendar has a property MinSupportedDateTime. If you want to get the week of the year from that date, then in some cases, you get an ArgumentOutOfRangeException although the date passed to the method is a valid one.

Calendar implementations affected are Hijri-, UmAlQura-, Hebrew-, Persian-, JulianCalendar, and all implementations of EastAsianLunisolarCalendar.

Another bug is in the JapaneseCalendar where the method GetYear() returns 0 for a specific date range.

It is highly possible that I didn't find all bugs regarding the calendar implementations, so if you set an unusual calendar like the JapaneseLunisolarCalendar, it can happen that exceptions are thrown.
Customization of the MonthControl

The rendering of the control is handled by a separate renderer class. If you want to customize the drawing, then you can implement your own renderer by deriving from the abstract base class or the default renderer.

That's also true for the color table class which implements IXmlSerializable.
Adjusting the MonthCalendar Control

One point you have to be aware of is that only non-neutral cultures can be set. The reason for that is, neutral cultures have no value for the DateTimeFormat property, which I need for different reasons.

To adjust the visible sub-month views, you can directly set the CalendarDimensions property, or use the designer.

Using the property ColorTable, you can adjust the used colors, including whether to use gradient colors and the gradient mode or not.
Adjusting the DatePicker Control

The date in the text box part of the picker is displayed using the short date pattern, which you can also adjust. This pattern is also used when parsing a date.

The picker control has the CheckDate event where you can check and set if the entered date is a valid one. If not valid, then the date is displayed using the colors of the InvalidBackColor and InvalidForeColor properties.

Another important property is ClosePickerOnDayClick. With it, you can adjust whether the picker closes when clicking on a day, regardless of whether the day is already selected or not.
Extended Bolded Dates

The DatePicker and MonthCalendar controls now have two new properties:

    BoldedDateCategoryCollection
    BoldedDatesCollection

The first collection holds the definitions of categories for bolded dates, and the second the bolded dates with corresponding category.

The original property for defining bold dates works as before.
Usage of Native Digits

Both controls have a new property :

    UseNativeDigits

This property converts any displayed number in the controls to a string containing the
NativeDigits from the current Culture.

If setting the current Culture to Persian (fa-IR) then this culture has not arabic numerals
but persian numerals.

For instance the persian date 01.01.1390 in the format dd/MM/yyyy would then be displayed as

۰۱ / ۰۱ / ۱۳۹۰

Let me show you an image if setting a datepicker's Culture and the CultureCalendar to Persian:

Image 3
 

 

Furthermore the manual input of an date uses the native digits too.

Please note that the displayed day names (probably the month names too) could be wrong,
because these values are taken from the .Net Framework CultureInfo classes, specifically the
Persian day names (in this case the shortest day names) are incorrect due to a bug/feature of
the Persian (fa-IR) CultureInfo class.
Events

The month control has the following important events:

    DateChanged: Occurs when the month or year is changed via the context menu
    DateClicked: Occurs when a day is clicked in the 'Day' selection mode
    DateSelected: Occurs when a date or date range is selected
    SelectionExtendEnd: Occurs when the selection extension ended

The date picker has an important event:

    ValueChanged: Occurs when the date value changes

Both controls have one event in common:

    ActiveDateChanged: Occurs when the mouse is over a date

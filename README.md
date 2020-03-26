# How to edit the ListView tapped item in Xamarin.Forms (SfListView) using DataForm


You can edit the tapped [ListViewItem](https://help.syncfusion.com/cr/cref_files/xamarin/Syncfusion.SfListView.XForms~Syncfusion.ListView.XForms.ListViewItem.html?) by binding the item to [DataForm](https://help.syncfusion.com/xamarin/dataform/getting-started?) in Xamarin.Forms [SfListView](https://help.syncfusion.com/xamarin/listview/overview?).

The following article explains you how to edit the listview item in another page.

https://www.syncfusion.com/kb/11286/how-to-edit-the-listview-tapped-item-in-xamarin-forms-sflistview-using-dataform 

**XAML**

ListView ItemTemplate defined with **Edit** button to edit the data
``` xml
<ContentPage xmlns:listView="clr-namespace:Syncfusion.ListView.XForms;assembly=Syncfusion.SfListView.XForms">
    <ContentPage.Content>
        <Grid>
           <listView:SfListView x:Name="listView" 
                           ItemsSource="{Binding contactsinfo}" >
                        <listView:SfListView.ItemTemplate>
                            <DataTemplate>
                                <StackLayout >
                                <Grid>
                                            <Image Source="{Binding ContactImage}" />
                                            <StackLayout Grid.Column="1">
                                                <Label Text="{Binding ContactName}"/>
                                                <Label Text="{Binding ContactNumber}"/>
                                            </StackLayout>
                                            <Grid Grid.Column="2">
                                                <Button x:Name="ZoomIn" Text="Edit" Command="{Binding Path=BindingContext.TapCommand, Source={x:Reference Name=listView}}" 
                                            CommandParameter="{Binding .}" />
                                            </Grid>
                                        </Grid>   
                                    </StackLayout>                                    
                            </DataTemplate>
                        </listView:SfListView.ItemTemplate>
           </listView:SfListView>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```
**XAML**

DataForm defined with TappedItem binding from ViewModel.
``` xml
<Grid>
    <dataForm:SfDataForm x:Name="dataForm" 
                         LabelPosition="Top" CommitMode="PropertyChanged"
                         DataObject="{Binding TappedItem}"/>
    <Button Text="Done" Command="{Binding DoneCommand}" Grid.Row="1"/>
</Grid>
```
**C#**

On edit, Update the edited item to **TappedItem** property and navigate to DataForm page
``` c#
public class ContactsViewModel : INotifyPropertyChanged
{
    private Contacts _tappedItem;
    public Command<object> TapCommand { get; set; }
    public Command<object> DoneCommand { get; set; }
 
    public Contacts TappedItem
    {
        get => _tappedItem;
        set {
            _tappedItem = value;
            OnPropertyChanged("TappedItem");
        }
    }
 
    public ContactsViewModel()
    {
        TapCommand = new Command<object>(ButtonTapped);
        DoneCommand = new Command<object>(DoneTapped);
    }
 
    private void DoneTapped(object obj)
    {
        App.Current.MainPage.Navigation.PopAsync();
    }
 
    private void ButtonTapped(object obj)
    {
        TappedItem = obj as Contacts;
        var dataForm = new EditPage();
        dataForm.BindingContext = this;
        App.Current.MainPage.Navigation.PushAsync(dataForm);
    }
}
```
**Output**

![ListViewPage](https://github.com/SyncfusionExamples/edit-item-listview-xamarin/blob/master/ScreenShots/ListViewPage.png)

![DataFormPage](https://github.com/SyncfusionExamples/edit-item-listview-xamarin/blob/master/ScreenShots/DataFormPage.png)

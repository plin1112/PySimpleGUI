
# The PySimpleGUI Cookbook

You will find all of these Recipes in a single Python file ([Demo_Cookbook.py](https://github.com/MikeTheWatchGuy/PySimpleGUI/blob/master/Demo_Cookbook.py)) located on the project's GitHub page.   This program will allow you to view the source code and the window that it produces.

You'll find that starting with a Recipe will give you a big jump-start on creating your custom GUI.  Copy and paste one of these Recipes and modify it to match your requirements.

## Simple Data Entry - Return Values As List
Same GUI screen except the return values are in a list instead of a dictionary and doesn't have initial values.

![super simple 2](https://user-images.githubusercontent.com/13696193/43934091-8100e29a-9c1b-11e8-8d0a-9bd2d13e6d8e.jpg)

    import PySimpleGUI as sg

    # Very basic form.  Return values as a list
    form = sg.FlexForm('Simple data entry form')  # begin with a blank form

    layout = [
              [sg.Text('Please enter your Name, Address, Phone')],
              [sg.Text('Name', size=(15, 1)), sg.InputText()],
              [sg.Text('Address', size=(15, 1)), sg.InputText()],
              [sg.Text('Phone', size=(15, 1)), sg.InputText()],
              [sg.Submit(), sg.Cancel()]
             ]

    button, values = form.LayoutAndRead(layout)

    print(button, values[0], values[1], values[2])

## Simple data entry - Return Values As Dictionary
A simple form with default values.  Results returned in a dictionary.  Does not use a context manager

![super simple 2](https://user-images.githubusercontent.com/13696193/43934091-8100e29a-9c1b-11e8-8d0a-9bd2d13e6d8e.jpg)

    import PySimpleGUI as sg

    # Very basic form.  Return values as a dictionary
    form = sg.FlexForm('Simple data entry form')  # begin with a blank form

    layout = [
              [sg.Text('Please enter your Name, Address, Phone')],
              [sg.Text('Name', size=(15, 1)), sg.InputText('name', key='name')],
              [sg.Text('Address', size=(15, 1)), sg.InputText('address', key='address')],
              [sg.Text('Phone', size=(15, 1)), sg.InputText('phone', key='phone')],
              [sg.Submit(), sg.Cancel()]
             ]

    button, values = form.LayoutAndRead(layout)

    print(button, values['name'], values['address'], values['phone'])

---------------------



-----------
## Simple File Browse
Browse for a filename that is populated into the input field.

![simple file browse](https://user-images.githubusercontent.com/13696193/43934539-d8bd9490-9c1d-11e8-927f-98b523776fcb.jpg)

    import PySimpleGUI as sg

    with sg.FlexForm('SHA-1 & 256 Hash') as form:
        form_rows = [[sg.Text('SHA-1 and SHA-256 Hashes for the file')],
                     [sg.InputText(), sg.FileBrowse()],
                     [sg.Submit(), sg.Cancel()]]
        (button, (source_filename,)) = form.LayoutAndShow(form_rows)

    print(button, source_filename)

--------------------------
## Add GUI to Front-End of Script
Quickly add a GUI allowing the user to browse for a filename if a filename is not supplied on the command line using this 1-line GUI. It's the best of both worlds.

![script front-end](https://user-images.githubusercontent.com/13696193/44756573-39e9c380-aaf9-11e8-97b4-6679f9f5bd46.jpg)


    import PySimpleGUI as sg
    import sys

    if len(sys.argv) == 1:
        button, (fname,) = sg.FlexForm('My Script').LayoutAndRead([[sg.T('Document to open')],
                                                               [sg.In(), sg.FileBrowse()],
                                                               [sg.Open(), sg.Cancel()]])
    else:
        fname = sys.argv[1]

    if not fname:
        sg.Popup("Cancel", "No filename supplied")
        raise SystemExit("Cancelling: no filename supplied")




--------------

## Compare 2 Files

Browse to get 2 file names that can be then compared.  Uses a context manager

![compare 2 files](https://user-images.githubusercontent.com/13696193/43934659-60dc5fbe-9c1e-11e8-8d2b-07c0e3b61892.jpg)

    import PySimpleGUI as sg

    with sg.FlexForm('File Compare') as form:
        form_rows = [[sg.Text('Enter 2 files to comare')],
                     [sg.Text('File 1', size=(8, 1)), sg.InputText(), sg.FileBrowse()],
                     [sg.Text('File 2', size=(8, 1)), sg.InputText(), sg.FileBrowse()],
                     [sg.Submit(), sg.Cancel()]]

        button, values = form.LayoutAndShow(form_rows)

    print(button, values)

---------------
## Nearly All Widgets with Green Color Theme with Context Manager
Example of nearly all of the widgets in a single form.  Uses a customized color scheme.  This recipe uses a context manager, the preferred method.

![green everything](https://user-images.githubusercontent.com/13696193/43937043-7d0794be-9c29-11e8-8591-31373ddd5c34.jpg)

    # Green & tan color scheme
    sg.SetOptions(background_color='#9FB8AD',
                  text_element_background_color='#9FB8AD',
                  element_background_color='#9FB8AD',
                  input_elements_background_color='#F7F3EC',
                  button_color=('white', '#475841'),
                  border_width=0,
                  slider_border_width=0,
                  progress_meter_border_depth=0,
                  scrollbar_color='#F7F3EC')

    with sg.FlexForm('Everything bagel', default_element_size=(40, 1)) as form:
        layout = [
            [sg.Text('All graphic widgets in one form!', size=(30, 1), font=("Helvetica", 25))],
            [sg.Text('Here is some text.... and a place to enter text')],
            [sg.InputText()],
            [sg.Checkbox('My first checkbox!'), sg.Checkbox('My second checkbox!', default=True)],
            [sg.Radio('My first Radio!     ', "RADIO1", default=True), sg.Radio('My second Radio!', "RADIO1")],
            [sg.Multiline(default_text='This is the default Text should you decide not to type anything', size=(35, 3)),
             sg.Multiline(default_text='A second multi-line', size=(35, 3))],
            [sg.InputCombo(('Combobox 1', 'Combobox 2'), size=(20, 3)),
             sg.Slider(range=(1, 100), orientation='h', size=(34, 20), default_value=85)],
            [sg.Listbox(values=('Listbox 1', 'Listbox 2', 'Listbox 3', 'Listbox 4'), size=(30, 3)),
             sg.Slider(range=(1, 100), orientation='v', size=(5, 20), default_value=25),
             sg.Slider(range=(1, 100), orientation='v', size=(5, 20), default_value=75),
             sg.Slider(range=(1, 100), orientation='v', size=(5, 20), default_value=10),
             sg.Spin(values=('Spin Box 1', '2', '3'), initial_value='Spin Box 1')],
            [sg.Text('_'  * 80)],
            [sg.Text('Your Folder', size=(15, 1), auto_size_text=False, justification='right'),
             sg.InputText('Default Folder'), sg.FolderBrowse()],
            [sg.Submit(), sg.Cancel(), sg.SimpleButton('Customized', button_color=('black', '#EDE5B7'))]]

        button, values = form.LayoutAndRead(layout)
-------------
### All Widgets No Context Manager

![green everything](https://user-images.githubusercontent.com/13696193/43937043-7d0794be-9c29-11e8-8591-31373ddd5c34.jpg)

    import PySimpleGUI as sg

    # Green & tan color scheme
    sg.SetOptions(background_color='#9FB8AD',
                  text_element_background_color='#9FB8AD',
                  element_background_color='#9FB8AD',
                  input_elements_background_color='#F7F3EC',
                  button_color=('white', '#475841'),
                  border_width=0,
                  slider_border_width=0,
                  progress_meter_border_depth=0,
                  scrollbar_color='#F7F3EC')

    form = sg.FlexForm('Everything bagel', default_element_size=(40, 1))
    layout = [
            [sg.Text('All graphic widgets in one form!', size=(30, 1), font=("Helvetica", 25))],
            [sg.Text('Here is some text.... and a place to enter text')],
            [sg.InputText('This is my text')],
            [sg.Checkbox('My first checkbox!'), sg.Checkbox('My second checkbox!', default=True)],
            [sg.Radio('My first Radio!     ', "RADIO1", default=True), sg.Radio('My second Radio!', "RADIO1")],
            [sg.Multiline(default_text='This is the default Text should you decide not to type anything', size=(35, 3)),
             sg.Multiline(default_text='A second multi-line', size=(35, 3))],
            [sg.InputCombo(('Combobox 1', 'Combobox 2'), size=(20, 3)),
             sg.Slider(range=(1, 100), orientation='h', size=(34, 20), default_value=85)],
            [sg.Listbox(values=('Listbox 1', 'Listbox 2', 'Listbox 3'), size=(30, 3)),
             sg.Slider(range=(1, 100), orientation='v', size=(5, 20), default_value=25),
             sg.Slider(range=(1, 100), orientation='v', size=(5, 20), default_value=75),
             sg.Slider(range=(1, 100), orientation='v', size=(5, 20), default_value=10),
             sg.Spin(values=('Spin Box 1', '2', '3'), initial_value='Spin Box 1')],
            [sg.Text('_'  * 80)],
            [sg.Text('Choose A Folder', size=(35, 1))],
            [sg.Text('Your Folder', size=(15, 1), auto_size_text=False, justification='right'),
             sg.InputText('Default Folder'), sg.FolderBrowse()],
            [sg.Submit(), sg.Cancel(), sg.SimpleButton('Customized', button_color=('white', '#7E6C92'))]
             ]

    button, values = form.LayoutAndRead(layout)

----
## Non-Blocking Form With Periodic Update
An async form that has a button read loop.  A Text Element is updated periodically with a running timer.  There is no context manager for this recipe because the loop that reads the form is likely to be some distance away from where the form was initialized.

![non-blocking](https://user-images.githubusercontent.com/13696193/43955295-70f6ac48-9c6d-11e8-8ea2-e6729ba9330c.jpg)

    import PySimpleGUI as sg
    import time

    form = sg.FlexForm('Running Timer')
    # create a text element that will be updated periodically
    text_element = sg.Text('', size=(10, 2), font=('Helvetica', 20), justification='center')

    form_rows = [[sg.Text('Stopwatch', size=(20,2), justification='center')],
                 [text_element],
                 [sg.T(' '  * 5), sg.ReadFormButton('Start/Stop', focus=True), sg.Quit()]]

    form.LayoutAndRead(form_rows, non_blocking=True)

    timer_running = True
    i = 0
    # loop to process user clicks
    while True:
        i += 1 * (timer_running is True)
        button, values = form.ReadNonBlocking()
        if values is None or button == 'Quit':  # if user closed the window using X or clicked Quit button
      break
     elif button == 'Start/Stop':
            timer_running = not timer_running
        text_element.Update('{:02d}:{:02d}.{:02d}'.format((i // 100) // 60, (i // 100) % 60, i % 100))

        time.sleep(.01)
        # if the loop finished then need to close the form for the user
    form.CloseNonBlockingForm()
    del (form)
----
## Async Form (Non-Blocking) with Context Manager
Like the previous recipe, this form is an async form.  The difference is that this form uses a context manager.

![non-blocking 2](https://user-images.githubusercontent.com/13696193/43955456-4d5d9ef8-9c6e-11e8-8598-80dddf8eba6f.jpg)

    import PySimpleGUI as sg
    import time

    with sg.FlexForm('Running Timer') as form:
        text_element = sg.Text('', size=(10, 2), font=('Helvetica', 20), text_color='red', justification='center')
        layout = [[sg.Text('Non blocking GUI with updates', justification='center')],
                  [text_element],
                  [sg.T(' '  * 15), sg.Quit()]]
        form.LayoutAndRead(layout, non_blocking=True)

        for i in range(1, 500):
            text_element.Update('{:02d}:{:02d}.{:02d}'.format((i // 100) // 60, (i // 100) % 60, i % 100))
            button, values = form.ReadNonBlocking()
            if values is None or button == 'Quit':  # if user closed the 				window using X
		      break
		    time.sleep(.01)
        else:
            # if the loop finished then need to close the form for the user
            form.CloseNonBlockingForm()
----
## Callback Function Simulation
The architecture of some programs works better with button callbacks instead of handling in-line.  While button callbacks are part of the PySimpleGUI implementation, they are not directly exposed to the caller.  The way to get the same result as callbacks is to simulate them with a recipe like this one.

![button callback 2](https://user-images.githubusercontent.com/13696193/43955588-e139ddc6-9c6e-11e8-8c78-c1c226b8d9b1.jpg)

    import PySimpleGUI as sg

    # This design pattern simulates button callbacks
    # Note that callbacks are NOT a part of the package's interface to the
    # caller intentionally.  The underlying implementation actually does use
    # tkinter callbacks.  They are simply hidden from the user.

    # The callback functions
    def button1():
        print('Button 1 callback')

    def button2():
        print('Button 2 callback')

    # Create a standard form
    form = sg.FlexForm('Button callback example')
    # Layout the design of the GUI
    layout = [[sg.Text('Please click a button')],
              [sg.ReadFormButton('1'), sg.ReadFormButton('2'), sg.Quit()]]
    # Show the form to the user
    form.Layout(layout)

    # Event loop. Read buttons, make callbacks
    while True:
        # Read the form
      button, value = form.Read()
        # Take appropriate action based on button
      if button == '1':
            button1()
        elif button == '2':
            button2()
        elif button =='Quit'  or button is None:
            break

    # All done!
    sg.PopupOk('Done')

-----
## Realtime Buttons (Good For Raspberry Pi)
This recipe implements a remote control interface for a robot.  There are 4 directions, forward, reverse, left, right.  When a button is clicked, PySimpleGUI immediately returns button events for as long as the buttons is held down.  When released, the button events stop.  This is an async/non-blocking form.

![robot control](https://user-images.githubusercontent.com/13696193/44006710-d227f23e-9e56-11e8-89a3-2be5b2726199.jpg)

    import PySimpleGUI as sg

    # Make a form, but don't use context manager
    form = sg.FlexForm('Robotics Remote Control', auto_size_text=True)

    form_rows = [[sg.Text('Robotics Remote Control')],
                 [sg.T(' '  * 10), sg.RealtimeButton('Forward')],
                 [sg.RealtimeButton('Left'), sg.T(' '  * 15), sg.RealtimeButton('Right')],
                 [sg.T(' '  * 10), sg.RealtimeButton('Reverse')],
                 [sg.T('')],
                 [sg.Quit(button_color=('black', 'orange'))]
                 ]

    form.LayoutAndRead(form_rows, non_blocking=True)

    #
    # Some place later in your code...
    # You need to perform a ReadNonBlocking on your form every now and then or
    # else it won't refresh.
    #
    # your program's main loop
    while (True):
        # This is the code that reads and updates your window
      button, values = form.ReadNonBlocking()
        if button is not None:
            print(button)
        if button == 'Quit'  or values is None:
            break

    form.CloseNonBlockingForm()

---------

## Easy Progress Meter
This recipe shows just how easy it is to add a progress meter to your code.

![progress meter 6](https://user-images.githubusercontent.com/13696193/43955982-73b33b38-9c70-11e8-8b07-cc1473a58a73.jpg)

    import PySimpleGUI as sg

    for i in range(1000):
        sg.EasyProgressMeter('Easy Meter Example', i+1, 1000)


-----
## Tabbed Form
Tabbed forms are **easy** to make and use in PySimpleGUI.  You simple may your layouts for each tab and then instead of `LayoutAndRead` you call `ShowTabbedForm`.  Results are returned as a list of form results.  Each tab acts like a single form.


![tabbed form](https://user-images.githubusercontent.com/13696193/43956352-cffa6564-9c71-11e8-971b-2b395a668bf3.jpg)

    import PySimpleGUI as sg

    with sg.FlexForm('', auto_size_text=True) as form:
        with sg.FlexForm('', auto_size_text=True) as form2:

            layout_tab_1 = [[sg.Text('First tab', size=(20, 1), font=('helvetica', 15))],
                            [sg.InputText(), sg.Text('Enter some info')],
                            [sg.Submit(button_color=('red', 'yellow')), sg.Cancel(button_color=('white', 'blue'))]]

            layout_tab_2 = [[sg.Text('Second Tab', size=(20, 1), font=('helvetica', 15))],
                            [sg.InputText(), sg.Text('Enter some info')],
                            [sg.Submit(button_color=('red', 'yellow')), sg.Cancel(button_color=('white', 'blue'))]]

            results = sg.ShowTabbedForm('Tabbed form example', (form, layout_tab_1, 'First Tab'),
                                        (form2, layout_tab_2,'Second Tab'))

    sg.Popup(results)
-----
## Button Graphics (Media Player)
Buttons can have PNG of GIF images on them.  This Media Player recipe requires 4 images in order to function correctly.  The background is set to the same color as the button background so that they blend together.

![media player](https://user-images.githubusercontent.com/13696193/43958418-5dd133f2-9c79-11e8-9432-0a67007e85ac.jpg)

    import PySimpleGUI as sg

    background = '#F0F0F0'
    # Set the backgrounds the same as the background on the buttons
    sg.SetOptions(background_color=background, element_background_color=background)
    # Images are located in a subfolder in the Demo Media Player.py folder
    image_pause = './ButtonGraphics/Pause.png'
    image_restart = './ButtonGraphics/Restart.png'
    image_next = './ButtonGraphics/Next.png'
    image_exit = './ButtonGraphics/Exit.png'

    # A text element that will be changed to display messages in the GUI
    TextElem = sg.Text('', size=(15, 2), font=("Helvetica", 14))

    # Open a form, note that context manager can't be used generally speaking for async forms
    form = sg.FlexForm('Media File Player', auto_size_text=True, default_element_size=(20, 1),
                       font=("Helvetica", 25))
    # define layout of the rows
    layout = [[sg.Text('Media File Player', size=(17, 1), font=("Helvetica", 25))],
              [TextElem],
              [sg.ReadFormButton('Restart Song', button_color=(background, background),
                                 image_filename=image_restart, image_size=(50, 50), image_subsample=2, border_width=0),
               sg.Text(' '  * 2),
               sg.ReadFormButton('Pause', button_color=(background, background),
                                 image_filename=image_pause, image_size=(50, 50), image_subsample=2, border_width=0),
               sg.Text(' '  * 2),
               sg.ReadFormButton('Next', button_color=(background, background),
                                 image_filename=image_next, image_size=(50, 50), image_subsample=2, border_width=0),
               sg.Text(' '  * 2),
               sg.Text(' '  * 2), sg.SimpleButton('Exit', button_color=(background, background),
                                                 image_filename=image_exit, image_size=(50, 50), image_subsample=2,
                                                 border_width=0)],
              [sg.Text('_'  * 30)],
              [sg.Text(' '  * 30)],
              [
                  sg.Slider(range=(-10, 10), default_value=0, size=(10, 20), orientation='vertical',
                            font=("Helvetica", 15)),
                  sg.Text(' '  * 2),
                  sg.Slider(range=(-10, 10), default_value=0, size=(10, 20), orientation='vertical',
                            font=("Helvetica", 15)),
                  sg.Text(' '  * 8),
                  sg.Slider(range=(-10, 10), default_value=0, size=(10, 20), orientation='vertical',
                            font=("Helvetica", 15))],
              [sg.Text('Bass', font=("Helvetica", 15), size=(6, 1)),
               sg.Text('Treble', font=("Helvetica", 15), size=(10, 1)),
               sg.Text('Volume', font=("Helvetica", 15), size=(7, 1))]

              ]

    # Call the same LayoutAndRead but indicate the form is non-blocking
    form.LayoutAndRead(layout, non_blocking=True)
    # Our event loop
    while (True):
        # Read the form (this call will not block)
      button, values = form.ReadNonBlocking()
        if button == 'Exit'  or values is None:
            break
      # If a button was pressed, display it on the GUI by updating the text element
      if button:
            TextElem.Update(button)
----
## Script Launcher - Persistent Form
This form doesn't close after button clicks.  To achieve this the buttons are specified as `sg.ReadFormButton` instead of `sg.SimpleButton`.   The exception to this is the EXIT button.  Clicking it will close the form.  This program will run commands and display the output in the scrollable window.

![launcher 2](https://user-images.githubusercontent.com/13696193/43958519-b30af218-9c79-11e8-88da-fadc69da818c.jpg)

    import PySimpleGUI as sg
    import subprocess

    def Launcher():

        form = sg.FlexForm('Script launcher')

        layout =  [
                    [sg.Text('Script output....', size=(40, 1))],
                    [sg.Output(size=(88, 20))],
                    [sg.ReadFormButton('script1'), sg.ReadFormButton('script2'), sg.SimpleButton('EXIT')],
                    [sg.Text('Manual command', size=(15,1)), sg.InputText(focus=True), sg.ReadFormButton('Run', bind_return_key=True)]
                  ]

        form.Layout(layout)

        # ---===--- Loop taking in user input and using it to query HowDoI --- #
      while True:
            (button, value) = form.Read()
            if button == 'EXIT'  or button is None:
                break                                 # exit button clicked
            if button == 'script1':
                ExecuteCommandSubprocess('pip','list')
            elif button == 'script2':
                ExecuteCommandSubprocess('python', '--version')
            elif button == 'Run':
                ExecuteCommandSubprocess(value[0])


    def ExecuteCommandSubprocess(command, *args):
        try:
            sp = subprocess.Popen([command,*args], shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
            out, err = sp.communicate()
            if out:
                print(out.decode("utf-8"))
            if err:
                print(err.decode("utf-8"))
        except: pass


    if __name__ == '__main__':
        Launcher()
----
## Machine Learning GUI
A standard non-blocking GUI with lots of inputs.

![machine learning green](https://user-images.githubusercontent.com/13696193/43979000-408b77ba-9cb7-11e8-9ffd-24c156767532.jpg)

    import PySimpleGUI as sg

    # Green & tan color scheme
    sg.SetOptions(background_color='#9FB8AD',
                  text_element_background_color='#9FB8AD',
                  element_background_color='#9FB8AD',
                  input_elements_background_color='#F7F3EC',
                  button_color=('white', '#475841'),
                  border_width=0,
                  slider_border_width=0,
                  progress_meter_border_depth=0,
                  scrollbar_color='#F7F3EC')

    sg.SetOptions(text_justification='right')

    form = sg.FlexForm('Machine Learning Front End', font=("Helvetica", 12))  # begin with a blank form

    layout = [[sg.Text('Machine Learning Command Line Parameters', font=('Helvetica', 16))],
              [sg.Text('Passes', size=(15, 1)), sg.Spin(values=[i for i in range(1, 1000)], initial_value=20, size=(6, 1)),
               sg.Text('Steps', size=(18, 1)), sg.Spin(values=[i for i in range(1, 1000)], initial_value=20, size=(6, 1))],
              [sg.Text('ooa', size=(15, 1)), sg.In(default_text='6', size=(10, 1)), sg.Text('nn', size=(15, 1)), sg.In(default_text='10', size=(10, 1))],
              [sg.Text('q', size=(15, 1)), sg.In(default_text='ff', size=(10, 1)), sg.Text('ngram', size=(15, 1)), sg.In(default_text='5', size=(10, 1))],
              [sg.Text('l', size=(15, 1)), sg.In(default_text='0.4', size=(10, 1)), sg.Text('Layers', size=(15, 1)), sg.Drop(values=('BatchNorm', 'other'),auto_size_text=True)],
              [sg.Text('_'  * 100, size=(65, 1))],
              [sg.Text('Flags', font=('Helvetica', 15), justification='left')],
              [sg.Checkbox('Normalize', size=(12, 1), default=True), sg.Checkbox('Verbose', size=(20, 1))],
              [sg.Checkbox('Cluster', size=(12, 1)), sg.Checkbox('Flush Output', size=(20, 1), default=True)],
              [sg.Checkbox('Write Results', size=(12, 1)), sg.Checkbox('Keep Intermediate Data', size=(20, 1))],
              [sg.Text('_'  * 100, size=(65, 1))],
              [sg.Text('Loss Functions', font=('Helvetica', 15), justification='left')],
              [sg.Radio('Cross-Entropy', 'loss', size=(12, 1)), sg.Radio('Logistic', 'loss', default=True, size=(12, 1))],
              [sg.Radio('Hinge', 'loss', size=(12, 1)), sg.Radio('Huber', 'loss', size=(12, 1))],
              [sg.Radio('Kullerback', 'loss', size=(12, 1)), sg.Radio('MAE(L1)', 'loss', size=(12, 1))],
              [sg.Radio('MSE(L2)', 'loss', size=(12, 1)), sg.Radio('MB(L0)', 'loss', size=(12, 1))],
              [sg.Submit(), sg.Cancel()]]

    button, values = form.LayoutAndRead(layout)

-------
## Custom Progress Meter / Progress Bar
Perhaps you don't want all the statistics that the EasyProgressMeter provides and want to create your own progress bar. Use this recipe to do just that.

![custom progress meter](https://user-images.githubusercontent.com/13696193/43982958-3393b23e-9cc6-11e8-8b49-e7f4890cbc4b.jpg)


    import PySimpleGUI as sg

    def CustomMeter():
        # create the progress bar element
      progress_bar = sg.ProgressBar(10000, orientation='h', size=(20,20))
        # layout the form
      layout = [[sg.Text('A custom progress meter')],
                  [progress_bar],
                  [sg.Cancel()]]

      # create the form
      form = sg.FlexForm('Custom Progress Meter')
        # display the form as a non-blocking form
      form.LayoutAndRead(layout, non_blocking=True)
        # loop that would normally do something useful
      for i in range(10000):
            # check to see if the cancel button was clicked and exit loop if clicked
	        button, values = form.ReadNonBlocking()
            if button == 'Cancel'  or values == None:
                break
      # update bar with loop value +1 so that bar eventually reaches the maximum
      progress_bar.UpdateBar(i+1)
        # done with loop... need to destroy the window as it's still open
      form.CloseNonBlockingForm()

   ----

## The One-Line GUI

For those of you into super-compact code, a complete customized GUI can be specified, shown, and received the results using a single line of Python code.  The way this is done is to combine the call to `FlexForm` and the call to `LayoutAndRead`.  `FlexForm` returns a `FlexForm` object which has the `LayoutAndRead` method.


![simple](https://user-images.githubusercontent.com/13696193/44227935-ecb53b80-a161-11e8-968b-b3f963404dec.jpg)


Instead of

    import PySimpleGUI as sg

    layout = [[sg.Text('Filename')],
              [sg.Input(), sg.FileBrowse()],
              [sg.OK(), sg.Cancel()] ]

    button, (number,) = sg.FlexForm('Get filename example').LayoutAndRead(layout)

you can write this line of code for the exact same result (OK, two lines with the import):

    import PySimpleGUI as sg

    button, (filename,) = sg.FlexForm('Get filename example'). LayoutAndRead([[sg.Text('Filename')], [sg.Input(), sg.FileBrowse()], [sg.OK(), sg.Cancel()] ])
--------------------
## Multiple Columns
Starting in version 2.9 (not yet released but you can get from current GitHub) you can use the Column Element.  A Column is required when you have a tall element to the left of smaller elements.

This example uses a Column.  There is a Listbox on the left that is 3 rows high.  To the right of it are 3 single rows of text and input. These 3 rows are in a Column Element.

To make it easier to see the Column in the window, the Column background has been shaded blue.  The code is wordier than normal due to the blue shading.  Each element in the column needs to have the color set to match blue background.

![snap0202](https://user-images.githubusercontent.com/13696193/44234671-27749f00-a175-11e8-9e66-a3fccf6c077e.jpg)


    import PySimpleGUI as sg

    # Demo of how columns work
    # Form has on row 1 a vertical slider followed by a COLUMN with 7 rows
    # Prior to the Column element, this layout was not possible
    # Columns layouts look identical to form layouts, they are a list of lists of elements.

    # sg.ChangeLookAndFeel('BlueMono')

    # Column layout
    col = [[sg.Text('col Row 1', text_color='white', background_color='blue')],
           [sg.Text('col Row 2', text_color='white', background_color='blue'), sg.Input('col input 1')],
           [sg.Text('col Row 3', text_color='white', background_color='blue'), sg.Input('col input 2')]]

    layout = [[sg.Listbox(values=('Listbox Item 1', 'Listbox Item 2', 'Listbox Item 3'), select_mode=sg.LISTBOX_SELECT_MODE_MULTIPLE, size=(20,3)), sg.Column(col, background_color='blue')],
              [sg.Input('Last input')],
              [sg.OK()]]

    # Display the form and get values
    # If you're willing to not use the "context manager" design pattern, then it's possible
    # to collapse the form display and read down to a single line of code.
    button, values = sg.FlexForm('Compact 1-line form with column').LayoutAndRead(layout)

    sg.Popup(button, values, line_width=200)


## Persistent Form With Text Element Updates

This simple program keep a form open, taking input values until the user terminates the program using the "X" button.

![math game](https://user-images.githubusercontent.com/13696193/44537842-c9444080-a6cd-11e8-94bc-6cdf1b765dd8.jpg)



    import PySimpleGUI as sg

    form = sg.FlexForm('Math')

    output = sg.Txt('', size=(8,1))

    layout = [ [sg.Txt('Enter values to calculate')],
               [sg.In(size=(8,1), key='numerator')],
               [sg.Txt('_'  * 10)],
               [sg.In(size=(8,1), key='denominator')],
               [output],
               [sg.ReadFormButton('Calculate', bind_return_key=True)]]

    form.Layout(layout)

    while True:
        button, values = form.Read()

        if button is not None:
            try:
                numerator = float(values['numerator'])
                denominator = float(values['denominator'])
                calc = numerator / denominator
            except:
                calc = 'Invalid'

            output.Update(calc)
        else:
            break



## tkinter Canvas Widget

The Canvas Element is one of the few tkinter objects that are directly accessible.  The tkinter Canvas widget itself can be retrieved from a Canvas Element like this:

    can = sg.Canvas(size=(100,100))
    tkcanvas = can.TKCanvas
    tkcanvas.create_oval(50, 50, 100, 100)


![canvas](https://user-images.githubusercontent.com/13696193/44632429-5266ac00-a948-11e8-9ee0-664103c40178.jpg)




    import PySimpleGUI as gui

    canvas = gui.Canvas(size=(100,100), background_color='red')

    layout = [
               [canvas],
               [gui.T('Change circle color to:'), gui.ReadFormButton('Red'), gui.ReadFormButton('Blue')]
               ]

    form = gui.FlexForm('Canvas test')
    form.Layout(layout)
    form.ReadNonBlocking()

    cir = canvas.TKCanvas.create_oval(50, 50, 100, 100)

    while True:
        button, values = form.Read()
        if button is None:
            break
        if button is 'Blue':
            canvas.TKCanvas.itemconfig(cir, fill = "Blue")
        elif button is 'Red':
            canvas.TKCanvas.itemconfig(cir, fill = "Red")


## Input Element Update

This Recipe implements a Raspberry Pi touchscreen based keypad entry.  As the digits are entered using the buttons, the Input Element above it is updated with the input digits.
There are a number of features used in this Recipe including:
* Default Element Size
* auto_size_buttons
* ReadFormButton
* Dictionary Return values
* Update of Elements in form (Input, Text)
* do_not_clear of Input Elements


![keypad 2](https://user-images.githubusercontent.com/13696193/44640891-57504d80-a992-11e8-93f4-4e97e586505e.jpg)



    import PySimpleGUI as g

    # g.SetOptions(button_color=g.COLOR_SYSTEM_DEFAULT)   # because some people like gray buttons

    # Demonstrates a number of PySimpleGUI features including:
    #   Default element size
    #   auto_size_buttons
    #   ReadFormButton
    #   Dictionary return values
    #   Update of elements in form (Text, Input)
    #   do_not_clear of Input elements

    # create the 2 Elements we want to control outside the form
    out_elem = g.Text('', size=(15, 1), font=('Helvetica', 18), text_color='red')
    in_elem = g.Input(size=(10, 1), do_not_clear=True, key='input')

    layout = [[g.Text('Enter Your Passcode')],
              [in_elem],
              [g.ReadFormButton('1'), g.ReadFormButton('2'), g.ReadFormButton('3')],
              [g.ReadFormButton('4'), g.ReadFormButton('5'), g.ReadFormButton('6')],
              [g.ReadFormButton('7'), g.ReadFormButton('8'), g.ReadFormButton('9')],
              [g.ReadFormButton('Submit'), g.ReadFormButton('0'), g.ReadFormButton('Clear')],
              [out_elem],
              ]

    form = g.FlexForm('Keypad', default_element_size=(5, 2), auto_size_buttons=False)
    form.Layout(layout)

    # Loop forever reading the form's values, updating the Input field
    keys_entered = ''
    while True:
        button, values = form.Read()  # read the form
        if button is None:  # if the X button clicked, just exit
            break
        if button is 'Clear':  # clear keys if clear button
            keys_entered = ''
        elif button in '1234567890':
            keys_entered = values['input']  # get what's been entered so far
            keys_entered += button  # add the new digit
        elif button is 'Submit':
            keys_entered = values['input']
            out_elem.Update(keys_entered)  # output the final string

        in_elem.Update(keys_entered)  # change the form to reflect current key string

## Animated Matplotlib Graph

Use the Canvas Element to create an animated graph.  The code is a bit tricky to follow, but if you know Matplotlib then this recipe shouldn't be too difficult to copy and modify.

![animated matplotlib](https://user-images.githubusercontent.com/13696193/44640937-91b9ea80-a992-11e8-9c1c-85ae74013679.jpg)


    from tkinter import *
    from random import randint
    import PySimpleGUI as g
    from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg, FigureCanvasAgg
    from matplotlib.figure import Figure
    import matplotlib.backends.tkagg as tkagg
    import tkinter as Tk


    def main():
        fig = Figure()

        ax = fig.add_subplot(111)
        ax.set_xlabel("X axis")
        ax.set_ylabel("Y axis")
        ax.grid()

        canvas_elem = g.Canvas(size=(640, 480))  # get the canvas we'll be drawing on

        layout = [[g.Text('Animated Matplotlib', size=(40, 1), justification='center', font='Helvetica 20')],
                  [canvas_elem],
                  [g.ReadFormButton('Exit', size=(10, 2), pad=((280, 0), 3), font='Helvetica 14')]]

        # create the form and show it without the plot
        form = g.FlexForm('Demo Application - Embedding Matplotlib In PySimpleGUI')
        form.Layout(layout)
        form.ReadNonBlocking()

        graph = FigureCanvasTkAgg(fig, master=canvas_elem.TKCanvas)
        canvas = canvas_elem.TKCanvas

        dpts = [randint(0, 10) for x in range(10000)]
        for i in range(len(dpts)):
            button, values = form.ReadNonBlocking()
            if button is 'Exit'  or values is None:
                exit(69)

            ax.cla()
            ax.grid()

            ax.plot(range(20), dpts[i:i + 20], color='purple')
            graph.draw()
            figure_x, figure_y, figure_w, figure_h = fig.bbox.bounds
            figure_w, figure_h = int(figure_w), int(figure_h)
            photo = Tk.PhotoImage(master=canvas, width=figure_w, height=figure_h)

            canvas.create_image(640 / 2, 480 / 2, image=photo)

            figure_canvas_agg = FigureCanvasAgg(fig)
            figure_canvas_agg.draw()

            tkagg.blit(photo, figure_canvas_agg.get_renderer()._renderer, colormode=2)
            # time.sleep(.1)

        if __name__ == '__main__':
            main()



## Tables

While there is no official support for "Tables" (e.g. there is no Table Element), it is possible to display information in a tabular way.  This only works for smaller tables because there is no way to scroll a window or a column element.  Until scrollable columns are implemented there is little use in creating  a Table Element.

![simpletable](https://user-images.githubusercontent.com/13696193/44960497-c667fd80-aece-11e8-988c-5fa7dfdb90a6.jpg)

    layout = [[sg.T('Table Test')]]

    for i in range(20):
        row = [sg.T(f'Row {i}  ', size=(10,1))]
        layout.append([sg.T(f'{i}{j}', size=(4,1), background_color='white', pad=(1,1)) for j in range(10)])

    sg.FlexForm('Table').LayoutAndRead(layout)

## Tight Layout

Saw this example layout written in tkinter and liked it so much I duplicated the interface.  It's "tight", clean, and has a nice dark look and feel.

This Recipe also contains code that implements the button interactions so that you'll have a template to build from.

In other GUI frameworks this program would be most likely "event driven" with callback functions being used to communicate button events.  The "event loop" would be handled by the GUI engine.  If code already existed that used a call-back mechanism, the loop in the example code below could simply call these callback functions directly based on the button text it receives in the form.Read call.

![timemanagement](https://user-images.githubusercontent.com/13696193/44996818-0f27c100-af78-11e8-8836-9ef6164efe3b.jpg)


    import PySimpleGUI as sg

    sg.ChangeLookAndFeel('Dark')
    sg.SetOptions(element_padding=(0, 0))

    StartButton = sg.ReadFormButton('Start', button_color=('white', 'black'))
    StopButton = sg.ReadFormButton('Stop', button_color=('gray34', 'black'))
    ResetButton = sg.ReadFormButton('Reset', button_color=('gray', 'firebrick3'))
    SubmitButton = sg.ReadFormButton('Submit', button_color=('gray34', 'springgreen4'))

    layout = [
        [sg.T('User:', pad=((3, 0), 0)), sg.OptionMenu(values=('User 1', 'User 2'), size=(20, 1)), sg.T('0', size=(8, 1))],
        [sg.T('Customer:', pad=((3, 0), 0)), sg.OptionMenu(values=('Customer 1', 'Customer 2'), size=(20, 1)),
         sg.T('1', size=(8, 1))],
        [sg.T('Notes:', pad=((3, 0), 0)), sg.In(size=(44, 1), background_color='white', text_color='black')],
        [StartButton, StopButton, ResetButton, SubmitButton]
        ]

    form = sg.FlexForm("Time Tracker", default_element_size=(12, 1), text_justification='r', auto_size_text=False,
                       auto_size_buttons=False,
                       default_button_element_size=(12, 1))
    form.Layout(layout)
    recording = have_data = False
    while True:
        button, values = form.Read()
        if button is None:
            exit(69)
        if button is 'Start':
            StartButton.Update(button_color=('gray34', 'black'))
            StopButton.Update(button_color=('white', 'black'))
            ResetButton.Update(button_color=('white', 'firebrick3'))
            recording = True
        elif button is 'Stop'  and recording:
            StopButton.Update(button_color=('gray34', 'black'))
            StartButton.Update(button_color=('white', 'black'))
            SubmitButton.Update(button_color=('white', 'springgreen4'))
            recording = False
            have_data = True
        elif button is 'Reset':
            StopButton.Update(button_color=('gray34', 'black'))
            StartButton.Update(button_color=('white', 'black'))
            SubmitButton.Update(button_color=('gray34', 'springgreen4'))
            ResetButton.Update(button_color=('gray34', 'firebrick3'))
            recording = False
            have_data = False
        elif button is 'Submit'  and have_data:
            StopButton.Update(button_color=('gray34', 'black'))
            StartButton.Update(button_color=('white', 'black'))
            SubmitButton.Update(button_color=('gray34', 'springgreen4'))
            ResetButton.Update(button_color=('gray34', 'firebrick3'))
            recording = False

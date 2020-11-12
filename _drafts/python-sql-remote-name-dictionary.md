Create data.world account
Access database (can be any!).
I chose [this Bible Names Dictionary][1] because I've previously built a command line dictionary tool and I'm planning to repurpose most of that code.

# Set up steps

There's a bit of set up at the beginning as per usual. First and foremost you should have a Python distribution installed, ideally Python3 since that's what I'm using on my machine. You can check whether you have this installed already from the terminal.
```
$ python3 --version
```
Set up steps for Python itself can be found [on their website][2].

Next we need to install the Python module provided by Datadotworld:
```
$ pip3 install datadotworld
```
(if you're on an earlier version of Python you'll need to use `pip` instead of `pip3`).

Once this is done you'll need to give Python the access to the data linked to your data.world profile. You can get the client token from the [integrations page][3] (`manage` section) and set it in the terminal:
```
$ export DW_AUTH_TOKEN=<YOUR_TOKEN>
```
More detailed instructions on setting up the datadotworld package can be found [on their website][4]

## Did it work?

Now it's all set up we can do a quick test to see whether it has been done correctly. First run Python3 from the command line:
```
$ python3
```
then try these commands:
```python
import datadotworld as dw
>>> results = dw.query(
...     'bradys/hitchcocks-bible-names-dictionary',
...     'SELECT * FROM hitchcocks_bible_dictionary')
>>> results_df = results.dataframe
>>> results_df
```
This should print the first and last few lines of the dataset we are looking to query for our dictionary: `hitchcocks-bible-names-dictionary`.
```
name                                          meaning
0           Aaron           a teacher; lofty; mountain of strength
1         Abaddon                                    the destroyer
2         Abagtha                         father of the wine-press
3           Abana                        made of stone; a building
4          Abarim                             passages; passengers
...           ...                                              ...
2618         Zuph  that beholds, observes, watches; roof; covering
2619          Zur                       stone; rock; that besieges
2620       Zuriel                          rock or strength of God
2621  Zurishaddai             the Almighty is my rock and strength
2622       Zuzims            the posts of a door; splendor; beauty

[2623 rows x 2 columns]

```

---
[1]: https://data.world/bradys/hitchcocks-bible-names-dictionary/workspace/file?filename=Hitchcocks+Bible+Dictionary "Hitchcocks Bible Names Dictionary – data.world"
[2]: https://www.python.org/downloads/ "Python – Downloads"
[3]: https://data.world/integrations/python "Python integration – data.world"
[4]: https://help.data.world/hc/en-us/articles/360039429733-Python-SDK "Setting up datadotworld package"

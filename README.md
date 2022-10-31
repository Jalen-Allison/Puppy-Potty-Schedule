# Live-Project

# Introduction
I recently did a two week live project at the end of my python course at The Tech Academy. I worked on building my own app that allows you to use CRUD functionality to keep track of your puppies potty schedule using the djgano framework. There were different stories describing the tasks that needed to be completed for the app through Azure Devops version control. As I progressed I built the front end and back end side of my app. It was a great opportunity to do live problem solving without a tutorial guiding every step of the way. The IDE used was PyCharm. I'm proud of the app I built, its design and functionality given the timeframe. While doing this two week sprint I had the chance to utilize project managemnt skills that will be used throughout my career.

Below are descriptions of the stroies that were worked on, along with the code used to complete them.

#CRUD
My code for my CRUD functions. They all have thier own links and pages dedicated to their respective functions.

# Story 1: Create a new app for the project, named appropriately for what you are tracking, and get it to display a home page with basic content.
<!-- Creates function to render home page-->
def puppy_home(request):
    return render(request, 'Puppy_Potty_Schedule/puppy_home.html')
    
# Story 2: Create a model for the collection item you will be tracking and add the ability to create a new item.
<!-- function to render built in form from my model puppy_add-->
def puppy_add(request):
    form = ScheduleForm(data=request.POST or None)
    if request.method == 'POST':
        if form.is_valid():
            form.save()
            return redirect('../time')
    content = {'form': form}
    return render(request, 'Puppy_Potty_Schedule/puppy_add.html', content)

# Story 3: Display information from the database in a page.
<!--function to fetch all objects created from form and render-->
def puppy_all(request):
    pups = Schedule.objects.all()
    content = {'pups': pups}
    return render(request, 'Puppy_Potty_Schedule/puppy_list.html', content)

# Story 4: Create a details page that will show the details of any single item from within the database, as selected by the user. Link this to the index page for each item.
<!--Display details page-->
def puppy_details(request, pk):
    details = get_object_or_404(Schedule, pk=pk)
    content = {'details': details}
    return render(request, 'Puppy_Potty_Schedule/puppy_details.html', content)
    
# Story 5: Allow for edits and delete functions to be done from the details page or from separate pages. Have confirmation before deleting.
<!--Delete and Update functions -->
def puppy_delete(request, pk):
    delete_post = get_object_or_404(Schedule, pk=pk)
    if request.method == "POST":
        delete_post.delete()
        return redirect('puppy_all')
    content = {'delete_post': delete_post}
    return render(request, 'Puppy_Potty_Schedule/puppy_delete.html', content)

def puppy_update(request, pk):
    update_post = get_object_or_404(Schedule, pk=pk)
    form = ScheduleForm(request.POST or None, instance=update_post)
    if request.method == 'POST':
        if form.is_valid():
            form.save()
            return redirect('puppy_all')
    content = {'form': form, 'update_post': update_post}
    return render(request, 'Puppy_Potty_Schedule/puppy_update.html', content)
    
    
 # Beautiful Soup
 
 Story 6: Create a new template for displaying information sourced from another website. Use Beautiful Soup to data scrape the site and find the relevant information.
<!--Beautiful Soup Parse through HTML-->
def puppy_bs(request):
    page = requests.get("https://www.akc.org/expert-advice/training/potty-training-your-puppy-timeline-and-tips/")
    soup = BeautifulSoup(page.content, 'html.parser')
    info = soup.find_all('p')[0].get_text()
    content = {"info": info}
    return render(request, 'Puppy_Potty_Schedule/puppy_bs.html', content)

<!--Beautiful Soup template linking to website to parse through html-->
{% extends "puppy_base.html" %}

{% block content %}
<h1>Beautiful Soup: </h1>
<h2>These helpful potty training tips were scraped from:<br><br><a target="_blank" href="https://www.akc.org/expert-advice/training/potty-training-your-puppy-timeline-and-tips//">"https://www.akc.org/expert-advice/training/potty-training-your-puppy-timeline-and-tips/":</a></h2>
<p>{{ info }}</p>
{% endblock %}
<!--End beautiful soup-->

# CSS for project
/* Makes background image fit the whole page */
body {
    background: url("../images/puppyb.jpeg") no-repeat center center fixed;
    -webkit-background-size: cover;
    -moz-background-size: cover;
    -o-background-size: cover;
    background-size: cover;
}

/* Add a transparent background to the top navigation */
.navbar {
  overflow: hidden;
  background-color: transparent;
}


/* Style the links inside the navigation bar */
.navbar a {
  float: left;
  color: #f2f2f2;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
  font-size: 25px;
}



/* Change the color of links on hover */
.navbar a:hover {
  background-color: gold;
  color: black;
}


h1 {
text-align: center;
color: gold;
margin: 2% auto;
text-shadow: 2px 2px 5px black;
font-size: 2em;
}

h2 {
text-align: center;
color: gold;
font-size: 1.5em;
}


.footer {
    position: fixed;
    text-transform: uppercase;
    left: 0;
    bottom: 0;
    width: 1950px;
    height: 40px;
    background-color: black;
    display: flex;
    justify-content: center;
    align-items: center;
}

.footer a {
    padding-bottom: 10px;
    text-decoration: none;
    text-size: 4em;
    font-weight: bold;
    background-image: linear-gradient(to left, white, gold);
    -webkit-background-clip: text;
    -moz-background-clip: text;
    background-clip: text;
    color: transparent;
}

form { /*styling the form layout */
    display: block;
    margin-left: auto;
    margin-right: auto;
    padding: 5px;
    width: 20%;
    background-color: gold;
    border: solid black 9px;
    text-align: left;
}

table {  /*styling the table layout */
    display: block;
    margin-left: auto;
    margin-right: auto;
    padding: 5px;
    width: 40%;
    background-color: gold;
    border: solid black 9px;
    text-align: left;
    border: 1px solid black;

}

table thead tr { /*label field*/
    background-color: transparent;
    color: black;
    text-align: center;
    font-weight: bold;
    border: 1px solid black;
}

table td { /*data field*/
    padding: 12px 15px;
    border: 1px solid black;
}

.create-container {
	opacity: 0.80;
	position: relative;
	width: auto;
	height: auto;
	border-radius: 8px;
	border: 2px solid gold;
	box-shadow: 4px 3px gold;
	margin-left: auto;
	margin-right: auto;
	margin-top: 10px;
	background-color: black;
	text-align: center;
	z-index: 1;
}

# Skills Learned
Working on a deadline and having daily standup taught me how to stay on schedule and adhere to the work schedule I set out to for the day or having to make the neccessary adjustments if my original timeline didnt go as planned. 
I also learned that nothing should be named the same thing as your model or the computer wont understand what you're trying to call to function. Plus, it also gets confusing on my end when I'm looking through my code and the same name is being used for different functions or labels especially when there is an error.

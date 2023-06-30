# Store Data with Python
With this code we are looking to query an API and store data into a MySQL database.

## Database Model
The database will have a very simple design, with the main entites being \_\_Books\_\_, \_\_Authors\_\_, and \_\_Themes\_\_.
A \_\_book\_\_ will consist of an ISBN, title, subtitle, and number of pages, an ISBN is a unique identifier for a book, so we'll use that as our primary key.
An \_\_author\_\_ will consist of the author's name and an \`id\` that will incriment every time a new author is inserted into the table.
Finally, a \_\_theme\_\_ has a similar schema as an author, it has the name of the theme, and an automatically incrementing \`id\`.

A book may have one to many authors, and as well, an author may have one to many books, this type of relationship is called a many-to-many relationship.
To map the \_\_book\_\_ and \_\_author\_\_ entities together, we use a link table, using a link table helps reduce duplicate data within a table.

**For example, let's look at the book:**
This book has four authors associated with it, if we had included an \`author\` column in the \_\_book\_\_ table, there would be multiple rows with the same title, isbn, subtitle, etc.
The same principle is applied to a \_\_book\_\_ and \_\_theme\_\_ relationship.

# Setup the Data for the API
Next, create a list of ISBNs to be queried against the API. The \`API_URL\` variable is the base URL for the OpenLibrary API, and afterwards, the parameters can be added in.

```
# the base URL to query the books API
API_URL = 'http://openlibrary.org/api/books'

# a list of ISBNs to be added to the catalog
isbn_list = [
    '978-0201853926',
    '0201558025',
    '978-1-93435-645-6',
    '9780199218462',
    '978-4915512377',
    '1593278551',
    '0811862151',
    '9780761174707',
    '1844834115',
    '9781408845646',
    '9780201896831',
    '0321534964',
    '1408855895',
    '0590353403',
]
```

# Query the OpenLibrary API
For each ISBN in the list, we build an \`isbn_payload\`.
This is the format in which the OpenLibrary API requires the \`bibkeys\` parameter to be formatted.
For example, using the book from before, the \`isbn_payload\` would be set to \`ISBN:0201633612\`.
Next, set up the parameters required for the API to retreive the book's data.
We set the \`format\` to return JSON, and the \`jscmd\` to \_\_data\_\_.
With all the parameters set, use the \`requests\` package to make the call to the API.

```
# cycle through the list and query the API
for isbn in isbn_list:
    # grab a fresh cursor
    cursor = db.cursor()

    # set up proper search parameters for the API
    isbn_payload = 'ISBN:%s' % isbn

    # set all parameters needed to make the request
    params = {
        'bibkeys':isbn_payload,
        'format':'json',
        'jscmd':'data'
    }

    # send the request
    r = requests.get(API_URL, params)
```
Go to http://openlibrary.org/api/books?bibkeys=ISBN:0201633612&format=json&jscmd=data in your web browser to view the raw JSON output from the API.

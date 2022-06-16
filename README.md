In the next lines of this post, we will be creating a simple react app with OMDB api (https://www.omdbapi.com/), the steps to get a key are very easy, just go to the website, click on API key, select free and type your email, check your inbox and thats it.

[You can contact me by telegram if you need to hire a Full Stack developer.](https://t.me/rtagliajs)

[You can also contact me by discord.](Appu#9136)


## Creating our Project
1. open your terminal and type following
2. npx create-react-app react-omdb
3. cd react-omdb
4. code .

The CSS used for this example is very simple, you can copy or download it from this link (https://github.com/rtagliaviaz/react-omdb-tut/blob/main/src/App.css) or make your own.

## Project file structure:

react-omdb/
├── node_modules/
├── public/
├── src/
│   ├── components/
│   ├── App.css
│   ├── App.js
│   └── index.css
│   └── index.js
└── package.json

## Packages
- axios

For this example we will only install axios as an additional dependency, to install it just open your terminal located in our project followed by `npm i axios`.

Finally to be able to start, we will reopen the console and we will execute the following command `npm start` to be able to see the changes that we will make throughout this post.

## Content table.

1. Creation of the Main.js component
2. API integration
3. Obtaining movie information
4. Creation and configuration of the modal to show the movie details
5. Pagination
6. Conclusion

---

### Let's get started!

1. Creation of the Main.js component

The first thing we will do is delete the files that we won't use, leaving our project as shown above in the project structure.

Then We will open our `App.js` file located inside src, we will remove the import of the logo, and we will modify our file leaving it as follows for the moment.

```js
import './App.css';

function App() {
  return (
    <div className="App">
      REACT OMDB
    </div>
  );
}

export default App;
```

After this we will go to our components folder inside the src folder (we will create it in case we haven't created it yet), inside components we will create a file called `Main.js` .

We will open our `Main.js`, we will begin importing the hooks `useState` and `useEffect` since we will use them later, and we will also import axios.

We will create a constant with the name of api where we will place our api, in this case 'https://www.omdbapi.com/?' and also a constant for apiKey to which we will assign our key.

At the moment we will only return a div with the name of our component.

```js
import React, {useState, useEffect} from 'react'
import axios from 'axios'

//api

const api = 'https://www.omdbapi.com/?'

//api key
const apiKey = 'apikey=18eaeb4f'

const Main = () => {
  
  return(
    <div>
      Main
    </div>
  )
}

export default Main
```

We will return to our `App.js` and we will import our `Main.js` component as shown in the code below, we will see that it already shows us the `Main.js` component.

```js
import React from 'react';
import './App.css'
//components
import Main from './components/Main'

function App() {
  return (
    <div className="App">
      REACT OMDB
      <Main />
    </div>
  );
}

export default App;
```

![Hello World](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/l4db0f2xa80dc83u4l8v.png)

2. API integration

Now we will return to our `Main.js` component  and we will make a form to be able to make a search with the movie name, and to make use of the `useState` hook. When clicking to the search button the function will be executed and will make a GET request to the api, and at the moment we will show the response by console.

```js
import React, { useState, useEffect } from "react";
import axios from "axios";

//api

const api = "https://www.omdbapi.com/?";

//api key
const apiKey = "apikey=18eaeb4f";

const Main = () => {
  const [name, setName] = useState("");

  //get response from API
  const getInfo = () => {
    axios
      .get(api + apiKey + `&s=${name}` + "&type=movie" + "&page=1")
      .then((res) => {
        if (res) {
          console.log(res.data);
        }
      });
  };

  //submit the title entered
  const handleSubmit = (e) => {
    e.preventDefault();
    getInfo();
  };

  return (
    <div>
      <form>
        <div className='searchBar'>
          <label htmlFor='name'></label>
          <input
            type='text'
            name='name'
            placeholder='movie name'
            onChange={(e) => setName(e.target.value)}
          />
          <button type='submit' onClick={(e) => handleSubmit(e)}>
            Search
          </button>
        </div>
      </form>
    </div>
  );
};

export default Main;
```

![api response](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/el80xhfzbhasxbefqgbx.png)

We are receiving an object, with the total results (which we will use later to create the pagination), and an array with the first 10 movies it found, with a couple of details such as the poster, the release date, the imdbID (which we will also use later), and many more.

We will use `useState` again, in this case with an array of movies with an empty initial state, and it will change when we get the results. And now in the return() we will be rendering the list of results if the array has elements, otherwise we will return `null`.

for each each title we will render a poster, the title, and a details button(that at the moment does nothing), and we will use the imdbId as the key.

```js
import React, { useState, useEffect } from "react";
import axios from "axios";

//api

const api = "https://www.omdbapi.com/?";

//api key
const apiKey = "apikey=18eaeb4f";

const Main = () => {
  const [name, setName] = useState("");
  const [movies, setMovies] = useState([])

  //get response from API
  const getInfo = () => {
    axios
      .get(api + apiKey + `&s=${name}` + "&type=movie" + "&page=1")
      .then((res) => {
        if (res) {
          setMovies(res.data.Search)
        }
      });
  };

  //submit the title entered
  const handleSubmit = (e) => {
    e.preventDefault();
    getInfo();
  };

  return (
    <div>
      <form>
        <div className='searchBar'>
          <label htmlFor='name'></label>
          <input
            type='text'
            name='name'
            placeholder='movie name'
            onChange={(e) => setName(e.target.value)}
          />
          <button type='submit' onClick={(e) => handleSubmit(e)}>
            Search
          </button>
        </div>
      </form>

      {movies ? 
      <div className="movies">
        {movies.map(movie => (
          <div key={movie.imdbID} className="movie">
            <img src={movie.Poster} alt=""/>
            <div className="movie-title">
              <p>{movie.Title}</p>
            </div>
            <button className="movie-detailsBtn" >Details</button>

          </div>))}
      </div> 
      : null}
    </div>
  );
};

export default Main;
```

![movies displayed](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/inf8ukhb0dmsd7pfu8v8.png)

3. Obtaining movie information

Now for the details button we will create a function named `getDetails`, which will be have and id as argument (imdbID of the title), 

We are gonna make another GET request to the api with the id, an it will return the movie information, at the moment we will show it with a `console.log()`

```js
import React, { useState, useEffect } from "react";
import axios from "axios";

//api
const api = "https://www.omdbapi.com/?";

//api key
const apiKey = "apikey=18eaeb4f";

const Main = () => {
  const [name, setName] = useState("");
  const [movies, setMovies] = useState([])

  //get response from API
  const getInfo = () => {
    axios
      .get(api + apiKey + `&s=${name}` + "&type=movie" + "&page=1")
      .then((res) => {
        if (res) {
          setMovies(res.data.Search)
        }
      });
  };



  //submit the title entered
  const handleSubmit = (e) => {
    e.preventDefault();
    getInfo();
  };

  return (
    <div>
      <form>
        <div className='searchBar'>
          <label htmlFor='name'></label>
          <input
            type='text'
            name='name'
            placeholder='movie name'
            onChange={(e) => setName(e.target.value)}
          />
          <button type='submit' onClick={(e) => handleSubmit(e)}>
            Search
          </button>
        </div>
      </form>

      {movies ? 
      <div className="movies">
        {movies.map(movie => (
          <div key={movie.imdbID} className="movie">
            <img src={movie.Poster} alt=""/>
            <div className="movie-title">
              <p>{movie.Title}</p>
            </div>
            <button className="movie-detailsBtn" >Details</button>

          </div>))}
      </div> 
      : null}
    </div>
  );
};

export default Main;
```

![movie details from api](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ygxce3ef5mnorekrsp7s.png)

Let's add a new state named `movieDetails` that will be initialized with an empty object, and will be updated with the movie details.

```js
import React, { useState, useEffect } from "react";
import axios from "axios";

//api

const api = "https://www.omdbapi.com/?";

//api key
const apiKey = "apikey=18eaeb4f";

const Main = () => {
  const [name, setName] = useState("");
  const [movies, setMovies] = useState([])
  const [movieDetails, setMovieDetails] = useState({})

  //get response from API
  const getInfo = () => {
    axios
      .get(api + apiKey + `&s=${name}` + "&type=movie" + "&page=1")
      .then((res) => {
        if (res) {
          setMovies(res.data.Search)
        }
      });
  };

  //get details
  const getDetails = (e, id) => {
    e.preventDefault()

    axios.get(api + apiKey + `&i=${id}`).then((res) => {
      if (res) {
        setMovieDetails(res.data)
      }
    })
  }

  //submit the title entered
  const handleSubmit = (e) => {
    e.preventDefault();
    getInfo();
  };

  return (
    <div>
      <form>
        <div className='searchBar'>
          <label htmlFor='name'></label>
          <input
            type='text'
            name='name'
            placeholder='movie name'
            onChange={(e) => setName(e.target.value)}
          />
          <button type='submit' onClick={(e) => handleSubmit(e)}>
            Search
          </button>
        </div>
      </form>

      {movies ? 
      <div className="movies">
        {movies.map(movie => (
          <div key={movie.imdbID} className="movie">
            <img src={movie.Poster} alt=""/>
            <div className="movie-title">
              <p>{movie.Title}</p>
            </div>
            <button className="movie-detailsBtn" 
            onClick={e => getDetails(e, movie.imdbID)}
            >Details</button>

          </div>))}
      </div> 
      : null}
    </div>
  );
};

export default Main;
```

4. Creation and configuration of the modal to show the movie details

To show the data, we will be using a moda that will be displayed every time we click the "details" button, in order to make this, we are gonna make a new file named `MovieModal.js` inside the components folder, by the moment will render this structure.

```js
const MovieModal = () => {
  return(
    <div className="modal display-block">
      <section className="modal-main">
        <div className="modal-body">
          
        </div>
        <button className="modal-closebtn" >Close</button>
      </section>
    </div>
  )
}

export default MovieModal
```

In order to open the modal lets go back to `Main.js`:

1- let's begin importing our `MovieModal.js` component.

2- let''s make a new state `$const [selectedId, setSelectedId] = useState(null)` .

3- let's create anoteher state to show the modal `$const [show, setShow] = useState(false)` it will be a boolean initialized with a `false` value.

4- We will manage our modal functionality with three functions.

5- In our `getDetails` function we will update the selectId state with the id passes as argument, then after gettin the response from the api we will open the modal with `showModal()` .

6- Finally in the `return()` of our component we will conditionally render the modal if `MovieDetails` isn't empty, if the selectedId is strictly equals to the imdbID of the movie and if show is true, otherwise it won't show it.

```js
import React, { useState, useEffect } from "react";
import axios from "axios";

//components
import MovieModal from "./MovieModal";

//api
const api = "https://www.omdbapi.com/?";

//api key
const apiKey = "apikey=18eaeb4f";

const Main = () => {
  const [name, setName] = useState("");
  const [movies, setMovies] = useState([])
  const [selectedId, setSelectedId] = useState(null)
  const [movieDetails, setMovieDetails] = useState({})

  //modal
  const [show, setShow] = useState(false)


  //modal config

  const showModal = () => {
    setShow(true)
  }

  const hideModal = () => {
    
    setShow(false)
    setMovieDetails()
  }

  const handleClose = () => {
    hideModal()
  }

  //get response from API
  const getInfo = () => {
    axios
      .get(api + apiKey + `&s=${name}` + "&type=movie" + "&page=1")
      .then((res) => {
        if (res) {
          setMovies(res.data.Search)
        }
      });
  };

  //get details
  const getDetails = (e, id) => {
    e.preventDefault()

    setSelectedId(id)
    axios.get(api + apiKey + `&i=${id}`).then((res) => {
      if (res) {
        setMovieDetails(res.data)
        showModal()
      }
    })
  }

  //submit the title entered
  const handleSubmit = (e) => {
    e.preventDefault();
    getInfo();
  };

  return (
    <div>
      <form>
        <div className='searchBar'>
          <label htmlFor='name'></label>
          <input
            type='text'
            name='name'
            placeholder='movie name'
            onChange={(e) => setName(e.target.value)}
          />
          <button type='submit' onClick={(e) => handleSubmit(e)}>
            Search
          </button>
        </div>
      </form>

      {movies ? 
      <div className="movies">
        {movies.map(movie => (
          <div key={movie.imdbID} className="movie">
            <img src={movie.Poster} alt=""/>
            <div className="movie-title">
              <p>{movie.Title}</p>
            </div>
            <button className="movie-detailsBtn" 
            onClick={e => getDetails(e, movie.imdbID)}
            >Details</button>

            {/* modal */}
            {movieDetails && (selectedId===movie.imdbID) && show ? 
              <MovieModal/> : 
              <div className="modal display-none"></div>
            }

          </div>))}
      </div> 
      : null}
    </div>
  );
};

export default Main;
```

![Modal is working](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/92h7dlc5imrlfir6lkog.png)

As you may have noticed, we are not displaying any information and we can't close it either. Let's go back to `Main.js`, go to the `return()` part and in the modal rendering, let's pass `handleclose` as a prop and also the details named as `movieInfo`.

```js
{/* modal */}
{movieDetails && (selectedId===movie.imdbID) && show ? 
  <MovieModal 
  movieInfo={movieDetails} 
  handleClose={handleClose}/> : 
  <div className="modal display-none"></div>
}
```

Let's go back to `MovieModal.js`, we will pass the `props` like this.

```js
const MovieModal = ({movieInfo, handleClose}) => {
  return(
    .
    .
    .
  )
}
export default MovieModal
```

Now lets modify the `return()` to return some data, and lets add the functionality to our button with the `handleClose` function when we click it.

```js
const MovieModal = ({ movieInfo, handleClose }) => {
  return (
    <div className='modal display-block'>
      <section className='modal-main'>
        <div className='modal-body'>
          <div className='modal-img'>
            <img src={movieInfo.Poster} alt='Poster' />
          </div>
        </div>
        <div className='modal-info'>
          <p>
            <b>Actors:</b> {movieInfo.Actors}
          </p>
          <p>
            <b>Genre:</b> {movieInfo.Genre}
          </p>
          <p>
            <b>Director:</b> {movieInfo.Director}
          </p>
          <p>
            <b>Released:</b> {movieInfo.Released}
          </p>
          <p>
            <b>Plot:</b> {movieInfo.Plot}
          </p>
        </div>
        <button className='modal-closebtn' onClick={handleClose}>
          Close
        </button>
      </section>
    </div>
  );
};

export default MovieModal;
```

![modal with details](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bnlxxz45jb43ojan2ocj.png)

5. Pagination

Let's create the pagination to view the rest of the results.

We need to go back to `Main.js`, if we remember when we make the GET request we obtain an object with a property named `totalResults`, this property is the key to make the pagination.

![total results](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vg15psxat6w1xijpylpk.png)

First we will add a new state, then we will modify our `getInfo` function to make an update to the `totalReuslts` state.

```js
const Main = () => {
  const [name, setName] = useState("");
  const [movies, setMovies] = useState([])
  const [selectedId, setSelectedId] = useState(null)
  const [movieDetails, setMovieDetails] = useState({})

  //modal
  const [show, setShow] = useState(false)

  //pagination
  const [totalResults, setTotalResults] = useState()
  .
  .
  .

  //get response from API
  const getInfo = () => {
    axios
      .get(api + apiKey + `&s=${name}` + "&type=movie" + "&page=1")
      .then((res) => {
        if (res) {
          setMovies(res.data.Search)
          setTotalResults(res.data.totalResults)
        }
      });
  };

  .
  .
  .
```

Thanks to `totalResults` now we can calculate the number of pages that will have the search that we perform, we will create a new state `const [numberOfPages, setNumberOfPages] = useState()` then we will build a function named `getNumberOfPages` to get the number of pages from our search.

In the function we will divide by 10 because we can only get 10 results per page, so we will use the remainder operator `%`, to verify if we are gettin left overs, in that case we will add an extra page. We will make use of `parseInt()` because `totalResults` is a string. Finally we will update `numberOfPages` with the number of pages

```js
.
.
.
//pagination
  const [totalResults, setTotalResults] = useState()
  const [numberOfPages, setNumberOfPages] = useState()
.
.
.

const getNumberOfPages = () => {
  if (totalResults % 10 > 0) {
    const numberOfpages = parseInt((totalResults / 10) + 1)
    setNumberOfPages(numberOfpages)
    return
  }
    const numberOfpages = parseInt(totalResults / 10)
    setNumberOfPages(numberOfpages)
}
```

Now we will create a state that will be updated with the current page (selected page) `const [currentPage, setCurrentPage] = useState()` . 

We're going to modify the `getInfo()` function to make it update the `currentPage` with the value of 1, when we do the first search, and we will start rendering the pagination.

If  we already have the number of pages, then we will render a div with the pagination, otherwise we won''t render it. if the previous page `currentPage -1` is equals to 0 we won't show it, otherwise we will show `currentPage - 1`, then the current page `currentPage`, lastly the next page `currentPage + 1` .

```js
.
.
.
//pagination
const [totalResults, setTotalResults] = useState()
const [numberOfPages, setNumberOfPages] = useState()
const [currentPage, setCurrentPage] = useState()
.
.
.

//get response from API
const getInfo = () => {
  axios
    .get(api + apiKey + `&s=${name}` + "&type=movie" + "&page=1")
    .then((res) => {
      if (res) {
        setMovies(res.data.Search);
        setTotalResults(res.data.totalResults);
        setCurrentPage(1)
      }
    });
};


return(
  {numberOfPages ? (
    <div className='pages'>
      {/* if prev page is 0 it wont show */}
      {currentPage - 1 === 0 ? null : (
        <b >{currentPage - 1}</b>
      )}
      <b  className='actualPage'>
        {currentPage}
      </b>
      <b >{currentPage + 1}</b>
    </div>
  ) : null}
)
 ```

![pagination](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/d0s7m5epwwjkesii6898.png)

For now we can only see the pagination but is not working, let's make it work.

1- We will use `useEffect()` and make it call `getNumbersOfPages()` .

2- Now we are going to modify `getInfo()` function and we will pass `pageNumber` as an argument, so if we have the `pageNumber` we will pass it, otherwise we will use `pageNumber` with 1 as a default value.

3- We will make an array of pages and we will fill it with a for loop, that will iterate `numberOfPages`, example ( if `numberOfPages` = 5, then will push 5 elements in the array ).

4- We will create a function named `goTo(pageNumber)` and we will pass `pageNumber` as an argument, after we call this function we will update the `currentPage` state with the selected page value `pageNumber`. Finally it will call the `getInfo` function .

5- We will modify our Pagination rendering, now when we click the page it will execute the `goTo` function with the selected page

```js
.
.
.

//get response from API
const getInfo = (pageNumber) => {
  if (pageNumber) {
    axios
      .get(
        api + apiKey + `&s=${name}` + "&type=movie" + `&page=${pageNumber}`
      )
      .then((res) => {
        if (res) {
          setMovies(res.data.Search);
          setTotalResults(res.data.totalResults);
        }
      });
    return;
  }
  axios
    .get(api + apiKey + `&s=${name}` + "&type=movie" + "&page=1")
    .then((res) => {
      if (res) {
        setMovies(res.data.Search);
        setTotalResults(res.data.totalResults);
        setCurrentPage(1);
      }
    });
};

//getnumberOFpageseffect
useEffect(() => {
  getNumberOfPages();
});

const pages = [];

for (let i = 1; i <= numberOfPages; i++) {
  pages.push(<p key={i} onClick={e => goTo(i)}>{i}</p>)
}

const goTo = (pageNumber) => {

  setCurrentPage(pageNumber)
  getInfo(pageNumber)
  window.scrollTo(0, 0)
}


return(
  .
  .
  .
  {numberOfPages ? (
    <div className='pages'>
      {/* if prev page is 0 it wont show */}
      {currentPage - 1 === 0 ? null : <b onClick={e => goTo(currentPage-1)}>{currentPage - 1}</b>}
      <b onClick={e => goTo(currentPage)}className='actualPage'>{currentPage}</b>
      <b onClick={e => goTo(currentPage+1)}>{currentPage + 1}</b>
    </div>
  ) : null}
)
```

This is our file after all the modifications.

```js
import React, { useState, useEffect } from "react";
import axios from "axios";

//components
import MovieModal from "./MovieModal";

//api
const api = "https://www.omdbapi.com/?";

//api key
const apiKey = "apikey=18eaeb4f";

const Main = () => {
  const [name, setName] = useState("");
  const [movies, setMovies] = useState([]);
  const [selectedId, setSelectedId] = useState(null);
  const [movieDetails, setMovieDetails] = useState({});

  //modal
  const [show, setShow] = useState(false);

  //pagination
  const [totalResults, setTotalResults] = useState(0);
  const [numberOfPages, setNumberOfPages] = useState();
  const [currentPage, setCurrentPage] = useState();

  const getNumberOfPages = () => {
    if (totalResults % 10 > 0) {
      const numberOfpages = parseInt(totalResults / 10 + 1);
      setNumberOfPages(numberOfpages);
      return;
    }
    const numberOfpages = parseInt(totalResults / 10);
    setNumberOfPages(numberOfpages);
  };

  //modal config
  const showModal = () => {
    setShow(true);
  };

  const hideModal = () => {
    setShow(false);
    setMovieDetails();
  };

  const handleClose = () => {
    hideModal();
  };

  //get response from API
  const getInfo = (pageNumber) => {
    if (pageNumber) {
      axios
        .get(
          api + apiKey + `&s=${name}` + "&type=movie" + `&page=${pageNumber}`
        )
        .then((res) => {
          if (res) {
            setMovies(res.data.Search);
            setTotalResults(res.data.totalResults);
          }
        });
      return;
    }
    axios
      .get(api + apiKey + `&s=${name}` + "&type=movie" + "&page=1")
      .then((res) => {
        if (res) {
          setMovies(res.data.Search);
          setTotalResults(res.data.totalResults);
          setCurrentPage(1);
        }
      });
  };

  //get details
  const getDetails = (e, id) => {
    e.preventDefault();

    setSelectedId(id);
    axios.get(api + apiKey + `&i=${id}`).then((res) => {
      if (res) {
        setMovieDetails(res.data);
        showModal();
      }
    });
  };

  //submit the title entered
  const handleSubmit = (e) => {
    e.preventDefault();
    getInfo();
  };

  //getnumberOFpageseffect
  useEffect(() => {
    getNumberOfPages();
  });

  const pages = [];

  for (let i = 1; i <= numberOfPages; i++) {
    pages.push(
      <p key={i} onClick={(e) => goTo(i)}>
        {i}
      </p>
    );
  }

  const goTo = (pageNumber) => {
    setCurrentPage(pageNumber);
    getInfo(pageNumber);
    window.scrollTo(0, 0);
  };

  return (
    <div>
      <form>
        <div className='searchBar'>
          <label htmlFor='name'></label>
          <input
            type='text'
            name='name'
            placeholder='movie name'
            onChange={(e) => setName(e.target.value)}
          />
          <button type='submit' onClick={(e) => handleSubmit(e)}>
            Search
          </button>
        </div>
      </form>

      {movies ? (
        <div className='movies'>
          {movies.map((movie) => (
            <div key={movie.imdbID} className='movie'>
              <img src={movie.Poster} alt='' />
              <div className='movie-title'>
                <p>{movie.Title}</p>
              </div>
              <button
                className='movie-detailsBtn'
                onClick={(e) => getDetails(e, movie.imdbID)}
              >
                Details
              </button>

              {/* modal */}
              {movieDetails && selectedId === movie.imdbID && show ? (
                <MovieModal
                  movieInfo={movieDetails}
                  handleClose={handleClose}
                />
              ) : (
                <div className='modal display-none'></div>
              )}
            </div>
          ))}
        </div>
      ) : null}

      {numberOfPages ? (
        <div className='pages'>
          {/* if prev page is 0 it wont show */}
          {currentPage - 1 === 0 ? null : (
            <b onClick={(e) => goTo(currentPage - 1)}>{currentPage - 1}</b>
          )}
          <b onClick={(e) => goTo(currentPage)} className='actualPage'>
            {currentPage}
          </b>
          <b onClick={(e) => goTo(currentPage + 1)}>{currentPage + 1}</b>
        </div>
      ) : null}
    </div>
  );
};

export default Main;
```

6. Conclusion

In this post we learned how to integrate an api to a react application in a simple way.

I really hope you have been able to follow the post without any trouble, otherwise i apologize, please leave me your doubts or comments.

Like i said before, the interface that i created for this example is very simple, it can be improved as well as the code, I encourage you to improve it and add more features.

[You can contact me by telegram if you need to hire a Full Stack developer.](https://t.me/rtagliajs)

[You can also contact me by discord.](Appu#9136)

[You can clone the repo if you want.](https://github.com/rtagliaviaz/react-omdb-tut)

Thanks for your time.

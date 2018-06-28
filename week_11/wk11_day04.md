## Week 11, day 04

What we covered today:

  - Three.js contd
  - Algorithms

___

#### Codealongs

- [Three.js 2](https://github.com/textchimp/wdi-27/tree/master/week11/3d-graphics-threejs)
- [Algorithms](https://github.com/textchimp/wdi-27/tree/master/week11/algorithms)
- [Nokogiri Web Scraper](https://github.com/GrantjHanrahan/nokogiri-pokemon-scraper)

___

### Algorithms

### _Binary Search_

[Here](https://www.youtube.com/watch?v=ube5EYFlFR0) is quite a good visualisation.
Binary Search is a really common one. Here are the steps:
- To find index of element e with certain value:
- Start with an array sorted in descending order.
  - In each step:
    - Pick the middle element of the array (m) and compare it to e.
    - If element values are equal, then return index of m.
    - If e is greater than me, then e must be in left subarray. If m is greater than e, e must be in the right subarray.
    - Repeat those steps on new subarray.

#### Other Search Algorithms
Performance varies depending on sequence characteristics (distribution)

- [Linear](https://en.wikipedia.org/wiki/Linear_search)
- [Interpolation search](https://en.wikipedia.org/wiki/Interpolation_search)

#### Graph Algorithms
Much data is actually connected nodes and vertices, which makes it much harder to find algorithms that compute solutions quickly.

A lot of research goes into solving graph problems.

One of them most common ways to navigate a graph, is by using a thing called a **Depth First Search (DFS).**

  - DFS progresses by expanding the first child node of the search tree that appears and thus going deeper and deeper until a goal node is found, or until it hits a node that has no children.

  - Then the search backtracks, returning to the most recent node it hasn’t finished exploring.

  - Other solutions for this kind of thing include:
    - [Depth-limited](https://en.wikipedia.org/wiki/Iterative_deepening_depth-first_search)
    - [Breadth-first-search](https://en.wikipedia.org/wiki/Breadth-first_search)
    - [A*](https://en.wikipedia.org/wiki/A*_search_algorithm)
    - [Hill climbing](https://en.wikipedia.org/wiki/Hill_climbing)
    - [Dijkstra](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm)

#### The Travelling Salesman Problem

  `Given a list of cities and the distances between each pair of cities, what is the shortest possible route that visits each city exactly once and returns to the origin city?`

  This is considered an NP-Hard problem (Non-deterministic Polynomial-time hard), meaning that all solutions are exponential.

  [Lots more algorithms.](https://en.wikipedia.org/wiki/List_of_algorithms)

#### Geometry: Detecting Collision

Don't detect the box around it, detect the actual content.

- [Tuts Plus](https://code.tutsplus.com/tutorials/quick-tip-collision-detection-between-circles--active-10523)
- [Wikipedia](https://en.wikipedia.org/wiki/Collision_detection)
- [Box2d.js](http://box2d-js.sourceforge.net/index2.html)
- [Gskinner](http://blog.gskinner.com/archives/2005/10/source_code_sha.html)

#### k Means Clustering

[Read this](http://vis4.net/blog/posts/map-symbol-clustering-k-means-vs-noverlap/) and [read this (better).](https://en.wikipedia.org/wiki/K-means_clustering)

#### Decision Trees

[Read this.](https://en.wikipedia.org/wiki/Decision_tree_learning)

#### Compression: Run Length Encoding

[Read this](http://pippin.gimp.org/image_processing/chap_dir.html)
[Read this](http://www.fileformat.info/mirror/egff/ch09_03.htm)
[Read this](https://en.wikipedia.org/wiki/Run-length_encoding)

#### Image Processing: Edge Detection
- [Demo](http://nerget.com/edgeDetection/)
- [Demo](http://www.barbafan.de/html5video?video=tron)
- [Read this](https://en.wikipedia.org/wiki/Edge_detection)

___

### Sort Algorithms

- [Sorting Algorithms Visualised](https://www.youtube.com/watch?v=kPRA0W1kECg)


#### _Bubble Sort_

```javascript
const bubbleSort = ( arr ) => {

  arr = arr.slice(); // This creates another copy of the array so we keep the original unchanged.
  // arr = [...arr]; This will work in the same way.
  let end = arr.length - 1;

  while( end > 0 ) {
    for( let i = 0; i < end; i++ ) {
      if( arr[ i ] > arr[ i + 1 ] ){

        // Swap to values
        let largeValue = arr[ i ];
        arr[ i ] = arr[ i + 1 ];
        arr[ i + 1 ] = largeValue;

      }
    }
    end--;
  }
  return arr;
};

const values = [];
for( let i = 0; i < 1000; i ++){
  values.push( Math.random() * 10000 );
};

console.log( bubbleSort( values ) );
```
___

#### Further Reading

- [Bogosort](https://en.wikipedia.org/wiki/Bogosort)
- [Sorting Algorithm - Wiki](https://en.wikipedia.org/wiki/Sorting_algorithm)
- [Search Algorithm - Wiki](https://en.wikipedia.org/wiki/Search_algorithm)
- [Well, this is more watching - Sorting Dance](https://www.youtube.com/watch?v=lyZQPjUT5B4)
- [Visualgo](https://visualgo.net/en)
- [Nokogiri](http://www.nokogiri.org/)
- [Risky business - is screen scraping legal?](https://www.communicationscouncil.org.au/public/content/ViewCategory.aspx?id=1413)

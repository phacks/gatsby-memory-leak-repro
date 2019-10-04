# Steps to repro [gatsbyjs#14525](https://github.com/gatsbyjs/gatsby/issues/14525)

## To see the memory leak in action

1. Clone the repo: `git clone https://github.com/phacks/gatsby-memory-leak-repro.git`
2. Install deps: `yarn` or `npm install`
3. Launch the development server: `gatsby develop`
4. Open Google Chrome on `localhost:8000`, open the Chrome Dev Tools, the “Memory” tab, and take a snapshot
5. Click on any of the ten links in the page, then take another snapshot
6. Filter on objects containing “detached”

Here’s what one should see:

_Before clicking on a link_:
![image](https://user-images.githubusercontent.com/2587348/59964627-5da44d00-9503-11e9-8db4-2acc3a72f0b4.png)

_After clicking on a link_:
![image](https://user-images.githubusercontent.com/2587348/59964640-790f5800-9503-11e9-8c84-bc16774ef506.png)

I searched for detached DOM Nodes following a personal hint of where the leak should be visible. Some other artifact of the leak may be visible elsewhere.

We can see that following the page transition, 9 `DetachedHTMLAnchorElement`, alongside many others, appear in the “detached” query.

7. To see another potential visualisation of the leak, click on the “three dots menu” on the upper-right hand of the dev tools, then “More Tools”, then “Performance Monitor”
8. Click the links to go back and forth between the two pages, and you can see the DOM Nodes counter (and JS heap size) increasing:

![Jun-24-2019 20-06-36](https://user-images.githubusercontent.com/2587348/60041515-e82fac80-96bb-11e9-8aba-e819843715fb.gif)

_Note: the superfluous DOM Nodes eventually get Garbage Collected. One can force this operation in the Memory tab of the Dev Tools._

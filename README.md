# ✒️ 文摘
Store some of my favorite words.

Inspired by [Dark495.me](https://dark495.me)

## Usage
This application will randomly display an excerpt, and you can browse through the content using the following methods:

### Keyboard Control
- **← ArrowLeft:** Previous excerpt
- **→ ArrowRight:** Next excerpt
- **R:** Random excerpt

### User Interface Control
At the bottom control bar, you can click the left and right arrows or the random button to browse excerpts.
- **Previous:** Click the left arrow `←` to view the previous excerpt
- **Random:** Click the random button `🔄` to get a random excerpt
- **Next:** Click the right arrow `→` to view the next excerpt

## Application Structure
- **Data Source:** Excerpt data is stored in the `./assets/data/excerpts.json` file.
- **Logic:** The `App.vue` file handles loading, refreshing excerpts, and keyboard and interface control logic.
- **Style:** Utilizes `Noto Serif SC` and `Instrument Serif` fonts and features a clean responsive design.

## License
The code uses [The Unlicense](https://choosealicense.com/licenses/unlicense/), and excerpts are attributed to the author.
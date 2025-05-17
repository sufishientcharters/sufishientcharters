# sufishientcharters

This repo has the content and configuration for the sufishientcharters.com web site. The tech stack:

- [asdf](https://asdf.run/) for managing the tools
- [Hugo](https://gohugo.io/) v0.147.3 (extended)
- [Node.js](https://nodejs.org/) v22.15.1
- A [custom fork](https://github.com/sufishientcharters/hugo-hero-theme) of the [Hero](https://www.zerostatic.io/theme/hugo-hero/) theme
- [FareHarbor](https://fareharbor.com/) for booking
- [Netlify](https://www.netlify.com/) for hosting, DNS, and form processing
- [Instagram](https://www.instagram.com/) for photos
- [Netlify Plugin Add Instagram](https://github.com/philhawksworth/netlify-plugin-add-instagram) for adding Instagram photos to the site

## Getting Started

### Prerequisites

1. [asdf](https://asdf.run/)
1. [git](https://git-scm.com/), version 2.13 or higher

### Steps

1. Clone this repo with the theme submodule:

```bash
git clone --recurse-submodules https://github.com/sufishientcharters/sufishientcharters.git
cd sufishientcharters
```

2. Install the tools:

```bash
asdf install
```

3. Install the dependencies:

```bash
npm install
```

3. Build the site:

```bash
hugo
```

4. If the site builds without error, you can run the local server:

```bash
hugo server
```

5. Open http://localhost:1313 in your browser. Congrats! You're now running the site locally.

## Development

The live site is built from `main`, so any changes should happen on a feature branch and be merged via PR. The site is actually split between two repos:

- https://github.com/sufishientcharters/hugo-hero-theme for the theme and layout
- https://github.com/sufishientcharters/sufishientcharters for the content and configuration

Consequently you may need to make changes to both repos.

Once a change is ready to go live:

1. Push your feature branch.
1. Create a PR.
1. Verify that the PR builds properly (Netlify will run checks).
1. Merge the PR into `main`.
1. Watch the deploy in Netlify.
1. Verify that the changes are now live on the site.

name: Generate Contribution Snake

on:
  push:
    branches:
      - master  # Trigger on push to master branch

jobs:
  generate-snake:
    runs-on: ubuntu-latest

    steps:
      # Check out the repository so the workflow can access it
      - uses: actions/checkout@v2

      # Generate the snake image
      - uses: Platane/snk@master
        id: snake-gif
        with:
          github_user_name: mishmanners  # Replace with your GitHub username
          gif_out_path: dist/github-contribution-grid-snake.gif  # Output GIF path
          svg_out_path: dist/github-contribution-grid-snake.svg    # Output SVG path

      # Show the status of the build (for debugging)
      - run: git status

      # Push changes to the repository
      - name: Push changes
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}  # Token for authentication
          branch: master  # Branch to push changes to
          force: true

      # Deploy the generated files to GitHub Pages
      - uses: crazy-max/ghaction-github-pages@v2.1.3
        with:
          target_branch: output  # Branch where the files will be pushed
          build_dir: dist  # Directory containing the generated files
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}  # Token for authentication

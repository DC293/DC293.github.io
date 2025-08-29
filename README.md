# David Cole - Sports Technology & Data Science Blog

This is my personal GitHub Pages website showcasing my work in sports technology, machine learning, and data science. The site serves as both a portfolio and a learning resource.

## 🏗️ Built With

- **Jekyll** - Static site generator
- **Minimal Mistakes Theme** - Clean, responsive theme
- **GitHub Pages** - Hosting platform

## 📁 Site Structure

```
├── _machine_learning/     # Machine learning tutorials and examples
├── _data-science/         # Data science projects and analysis
├── _puzzles/             # Coding challenges and solutions
├── _pages/               # Main site pages (Home, About, etc.)
├── assets/images/        # Images and media files
└── _sass/               # Custom styling
```

## 🎯 Content Sections

### Machine Learning
Comprehensive tutorials covering fundamental ML concepts:
- Linear & Logistic Regression
- Decision Trees & Random Forests
- Support Vector Machines
- K-Nearest Neighbors
- Naive Bayes Classifier
- Evaluation Metrics

### Data Science
Practical applications and insights in sports analytics:
- Data cleaning and preprocessing
- Statistical analysis
- Visualization techniques

### Coding Puzzles
Solutions to programming challenges:
- Advent of Code 2023 solutions
- Algorithm implementations
- Problem-solving approaches

## 🔧 Recent Improvements

### Navigation Fix
- **Issue**: Previous/Next buttons weren't following the correct post order
- **Solution**: Created custom navigation template (`_includes/post-navigation.html`) that respects the `weight` field for proper ordering
- **Implementation**: Updated all collection pages to use `sort_by: weight`

### SEO Enhancements
- Added comprehensive meta descriptions
- Improved site title and subtitle
- Enhanced author information
- Added social media meta tags

### Visual Improvements
- Enhanced grid layouts with hover effects
- Improved navigation styling
- Better responsive design
- Code block styling improvements

### Content Updates
- Enhanced About page with detailed information
- Improved Data Science page description
- Better organization of content sections

## 🚀 Local Development

To run the site locally:

```bash
# Install dependencies
bundle install

# Start local server
bundle exec jekyll serve

# Visit http://localhost:4000
```

## 📝 Adding New Content

### New Blog Post
1. Create a new `.md` file in the appropriate collection folder
2. Add front matter with required fields:
   ```yaml
   ---
   layout: single
   weight: [number]  # For ordering
   title: "Your Title"
   excerpt: "Brief description"
   header:
     overlay_image: assets/images/your-image.png
     overlay_filter: 0.5
   toc: true
   categories: [collection_name]
   author_profile: true
   ---
   ```

### New Collection
1. Add collection to `_config.yml`:
   ```yaml
   collections:
     your_collection:
       output: true
       permalink: /:collection/:path/
   ```
2. Create collection folder: `_your_collection/`
3. Create collection page in `_pages/`

## 🎨 Customization

### Styling
- Custom CSS in `_sass/_custom.scss`
- Theme customization in `_config.yml`
- Responsive design improvements

### Navigation
- Main navigation in `_data/navigation.yaml`
- Post navigation uses custom template for proper ordering

## 📊 Performance

- Optimized images and assets
- Responsive design for all devices
- Fast loading times with Jekyll static generation

## 🤝 Contributing

This is a personal blog, but suggestions and feedback are welcome! Feel free to:
- Report bugs or issues
- Suggest improvements
- Share ideas for new content

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

**David Cole** - Research and Insights Manager at ITF, specializing in sports technology and data science.

# System Design Shapes

**Interactive visualization comparing 20 common system design patterns across 9 architectural dimensions**

An educational tool for system design interview preparation that visualizes how different systems (URL shorteners, payment systems, social media feeds, etc.) compare across critical architecture dimensions like scale, consistency, latency, and more.

## 🌐 Live Demo

**[View Live Application →](https://tslava.github.io/system-design-shapes/)**

## 🎯 Features

- **20 Real-World Systems**: From URL shorteners to stock trading platforms
- **9 Architecture Dimensions**: Scale, Read/Write Ratio, Consistency, Latency, Data Volume, Failure Tolerance, Geo Distribution, Cost Pressure, and Change Velocity
- **Interactive Radar Chart**: Visual comparison of multiple systems simultaneously with D3.js-powered calculations
- **Architecture Patterns**: Detailed patterns and 360+ tool recommendations for each dimension
- **Smart Theme Detection**: Automatically matches your system's light/dark mode preference
- **Pre-loaded Example**: URL Shortener pre-selected to demonstrate functionality immediately
- **Fully Responsive**: Optimized for mobile, tablet, and desktop with adaptive layouts
- **Zero Dependencies**: Single HTML file with no build step required

## 🚀 Quick Start

### Option 1: Use the Live Demo

Simply visit the [live demo link](#) and start exploring!

### Option 2: Run Locally

1. Clone this repository:
   ```bash
   git clone https://github.com/tslava/system-design-shapes.git
   cd system-design-shapes
   ```

2. Open `index.html` in your browser:
   ```bash
   open index.html  # macOS
   # or
   xdg-open index.html  # Linux
   # or just double-click index.html in Windows
   ```

That's it! No installation, no build process, no dependencies.

## 📖 Usage

1. **Quick Start**: The app loads with URL Shortener pre-selected as an example
2. **Select Systems**: Click on system pills at the top to add them to the comparison (multi-select enabled)
3. **View Radar Chart**: See how selected systems compare across all 9 dimensions
4. **Explore Patterns**: Scroll down to view detailed architecture patterns, tools, and recommendations for each system
5. **Toggle Theme**: The app automatically matches your system theme, or manually toggle with the sun/moon icon
6. **Clear Selection**: Click "Clear All" to reset and start a new comparison

## 🎨 Customization

### Editing Systems Data

All data is contained in the single `index.html` file. To modify systems or add new ones:

1. Open `index.html` in a text editor
2. Find the `SYSTEMS` array (around line 200)
3. Each system has:
   - `id`: Unique identifier
   - `name`: Display name
   - `icon`: Emoji icon
   - `color`: Hex color code
   - `examples`: Real-world examples
   - `scores`: Object with scores (1-5) for each of the 9 dimensions
   - `patterns`: Array of 9 pattern objects, each containing:
     - `dimension`: Key matching one of the 9 dimensions
     - `text`: 1-2 sentence pattern description
     - `tools`: Array of tools with `name`, `url`, and `rationale`

Example system structure:
```javascript
{
    id: 'my-system',
    name: 'My System',
    icon: '🚀',
    color: '#3b82f6',
    examples: 'Example1, Example2',
    scores: {
        scale: 4,
        readWrite: 3,
        consistency: 5,
        latency: 4,
        dataVolume: 3,
        failureTolerance: 5,
        geoDistribution: 3,
        costPressure: 2,
        changeVelocity: 3
    },
    patterns: [
        {
            dimension: 'scale',
            text: 'Description of how this system handles scale...',
            tools: [
                {
                    name: 'Redis',
                    url: 'https://redis.io/',
                    rationale: 'Why this tool is recommended'
                }
            ]
        },
        // ... 8 more patterns for other dimensions
    ]
}
```

### Modifying Dimensions

To change the 9 dimensions, edit the `DIMENSIONS` array (around line 100):

```javascript
const DIMENSIONS = [
    { key: 'dimension-key', label: 'Display Name', icon: '📊', description: 'Description' },
    // ...
];
```

## 📊 The 9 Dimensions Explained

1. **Scale (📊)**: Expected number of users/requests
2. **Read/Write Ratio (⚖️)**: Balance between read and write operations
3. **Consistency (🔒)**: Data consistency requirements (strong vs eventual)
4. **Latency (⚡)**: Response time requirements
5. **Data Volume (💾)**: Amount of data to store/process
6. **Failure Tolerance (🛡️)**: Resilience to failures and uptime requirements
7. **Geo Distribution (🌍)**: Geographic spread of users and data
8. **Cost Pressure (💰)**: Budget constraints and cost optimization needs
9. **Change Velocity (🚀)**: Rate of feature changes and deployment frequency

## 🛠️ Technical Stack

- **React 19.2.4** - UI framework
- **D3.js 7.9.0** - Radar chart calculations
- **Babel Standalone 7.28.6** - JSX transformation
- **Pure CSS** - Styling with CSS-in-JS

All loaded from CDNs, no build step required!

## 📦 Deployment

This project auto-deploys to GitHub Pages on every push to `main`.

### Setting Up GitHub Pages

1. Go to your repository Settings
2. Navigate to Pages section
3. Under "Source", select "GitHub Actions"
4. Push to `main` branch - the site will deploy automatically!

The deployment workflow is defined in `.github/workflows/deploy.yml`.

## 🤝 Contributing

Contributions are welcome! To contribute:

1. Fork this repository
2. Edit `index.html` with your changes
3. Test locally by opening in a browser
4. Submit a pull request with a clear description of your changes

Ideas for contributions:
- Add more systems (microservices, blockchain, etc.)
- Improve architecture pattern descriptions
- Add more tool recommendations
- Enhance mobile responsiveness
- Add animations and transitions

## 📚 Educational Context

This project is based on the "9 Questions for System Design Interviews" framework by [@rauljuncoV](https://twitter.com/rauljuncoV). The scores and patterns are approximate and meant for interview preparation and learning, not as absolute architectural truth.

Use this tool to:
- Prepare for system design interviews
- Understand trade-offs between different architectural approaches
- Learn common patterns for specific system types
- Discover tools and technologies for different use cases

## ⚠️ Disclaimer

The architecture scores, patterns, and tool recommendations in this application are simplified for educational purposes. Real-world system design involves many more nuances, trade-offs, and context-specific decisions. Always consider your specific requirements, constraints, and use cases when designing systems.

## 📄 License

MIT License - feel free to use this project for learning, teaching, or any other purpose!

## 🙏 Acknowledgments

- Framework and methodology by [@rauljuncoV](https://twitter.com/rauljuncoV)
- Built with ❤️ for the system design community

---

**Made for system design interview preparation** | **No build tools, just open and learn!**

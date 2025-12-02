# Capstone_Project
agents-intensive-capstone-project

# Local Crisis Resource Navigator - Setup Guide

## Prerequisites

- Python 3.8 or higher
- Node.js 16+ (for React frontend)
- Anthropic API key

## Installation

### 1. Backend Setup

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On Mac/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Set environment variable
export ANTHROPIC_API_KEY="your-api-key-here"
# On Windows:
set ANTHROPIC_API_KEY=your-api-key-here
```

### 2. Requirements.txt

```txt
anthropic>=0.40.0
flask>=3.0.0
flask-cors>=4.0.0
requests>=2.31.0
python-dotenv>=1.0.0
beautifulsoup4>=4.12.0
playwright>=1.40.0
```

### 3. Frontend Setup (React)

The React UI is already provided as an artifact. To integrate with the backend:

1. Update the API endpoint in the React component to point to your Flask server
2. Or use the provided UI directly in Claude.ai as an artifact

## Running the Application

### Option 1: CLI Interface (Recommended for testing)

```bash
python crisis_navigator.py
```

This runs an interactive command-line interface where you can test the agent system.

### Option 2: Flask API + React Frontend

**Terminal 1 - Start Backend:**
```bash
python -c "from crisis_navigator import create_flask_app; app = create_flask_app(); app.run(debug=True, port=5000)"
```

**Terminal 2 - Start Frontend:**
```bash
# If using separate React project
npm install
npm start
```

The frontend will be available at `http://localhost:3000`

## Usage Examples

### Example 1: Domestic Violence Crisis

```
YOU: I need to leave my apartment tonight. I'm scared and have nowhere to go. I'm in Austin, Texas.

NAVIGATOR: [Searches and provides 2-3 Austin DV shelters with phone numbers, addresses, and next steps]
```

### Example 2: Homelessness

```
YOU: I just lost my housing and need a place to sleep. I'm in Portland and have two kids.

NAVIGATOR: [Finds family shelters in Portland with immediate availability]
```

### Example 3: Mental Health Crisis

```
YOU: I'm having really dark thoughts and don't know what to do.

NAVIGATOR: [Immediately provides 988 hotline and local crisis resources]
```

## Architecture Overview

```
User Input
    ↓
IntakeAgent (context gathering)
    ↓
ResourceDiscoveryAgent (web search)
    ↓
VerificationAgent (validate resources)
    ↓
MatchingAgent (prioritize by relevance)
    ↓
ActionAgent (generate action plan)
    ↓
Response to User
```

## Key Features

1. **Multi-Agent System**: Specialized agents for intake, discovery, verification, matching, and action planning
2. **Real-time Web Search**: Uses Claude's web search capability to find current resources
3. **Context Management**: Maintains conversation state and user context
4. **Trauma-Informed**: Empathetic prompting and crisis-appropriate language
5. **Safety First**: Always includes emergency hotlines as fallback

## Customization

### Adding New Crisis Types

Edit the `CrisisType` enum in `crisis_navigator.py`:

```python
class CrisisType(Enum):
    # ... existing types
    YOUR_NEW_TYPE = "your_new_type"
```

Add search queries in `ResourceDiscoveryAgent._build_search_queries()`.

### Adding Local Resource Databases

Extend `ResourceDiscoveryAgent._search_web()` to query:
- 211 APIs
- Local government databases
- Nonprofit APIs

### Improving Verification

Enhance `VerificationAgent.verify_resources()` to:
- Call shelters to check real-time availability
- Parse capacity dashboards
- Check recent social media mentions

## Testing

```bash
# Test with sample scenarios
python -m pytest tests/

# Or manually test:
python crisis_navigator.py
```

Test cases:
1. DV survivor needs immediate shelter
2. Person experiencing homelessness, no transportation
3. Mental health crisis with suicide ideation
4. Addiction crisis needing detox
5. Family needing food assistance

## Production Considerations

1. **API Rate Limiting**: Implement caching for frequently searched locations
2. **Session Management**: Use Redis for multi-user sessions
3. **Database**: Store verified resources in PostgreSQL
4. **Monitoring**: Log all conversations (anonymized) for quality improvement
5. **Security**: HTTPS only, no PII storage, encrypted sessions
6. **Compliance**: HIPAA compliance if handling health data

## Deployment Options

### Heroku
```bash
heroku create crisis-navigator
heroku config:set ANTHROPIC_API_KEY=your-key
git push heroku main
```

### AWS Lambda
Use AWS Lambda + API Gateway for serverless deployment

### Docker
```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "crisis_navigator.py"]
```

## Legal & Ethical Considerations

- **Not a replacement for professional help**: Always direct to licensed professionals
- **Privacy**: Never store personal crisis details
- **Disclaimer**: Include clear disclaimer that this is informational only
- **Emergency protocol**: Always prioritize 911/988 for life-threatening situations

## Contributing

This is a capstone project, but improvements welcome:
- Add more crisis types
- Integrate real-time availability APIs
- Multi-language support
- SMS/voice interface
- Community feedback loops

## License

MIT License - Free to use and modify

## Support

For questions about this project:
- Review the code comments
- Check Claude's documentation: https://docs.anthropic.com
- Test with the CLI interface first

---

**Remember**: This tool is designed to help people in crisis. Handle with care, test thoroughly, and prioritize safety above all else.

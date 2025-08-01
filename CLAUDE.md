# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with n8n workflows in this repository.

## Repository Overview

This is the official n8n workflows repository for Everlab, containing all workflow automation as code. The repository serves as version control for n8n workflows, enabling collaboration, backup, and infrastructure as code practices.

## Key Context

### Organization
- **Company**: Everlab - Healthcare technology company
- **Primary Use Cases**: Bug tracking automation, customer support integration, HR notifications, healthcare provider onboarding
- **n8n Instance**: everlab.app.n8n.cloud

### Workflow Categories
1. **Bug Management**: Automated triaging from Slack/Front to Linear
2. **Communication**: AI-powered notification filtering and routing
3. **Healthcare Operations**: Doctor onboarding, compliance tracking
4. **Quality Assurance**: Automated monitoring and reporting

## Working with Workflows

### When Creating New Workflows

1. **Naming Convention**: 
   - Use format: `source-destination-purpose.json`
   - Example: `slack-linear-bug-triaging.json`

2. **Required Metadata**:
   - Clear trigger definition
   - Integration points documented
   - Error handling implemented
   - Credentials referenced (not stored)

3. **AI Integration Pattern**:
   - Most workflows use Claude for classification/analysis
   - Standard pattern: Trigger → AI Analysis → Conditional Logic → Action
   - Always include fallback for AI failures

### When Modifying Existing Workflows

1. **Check Dependencies**:
   - Review which integrations are used
   - Verify field mappings are current
   - Test with sample data before deployment

2. **Common Modifications**:
   - Adding new classification categories
   - Updating Slack channels or Linear teams
   - Adjusting AI prompts for better accuracy
   - Adding error notifications

## Technical Patterns

### Standard Workflow Structure
```
Trigger (Webhook/Schedule/Email)
  ↓
Data Extraction/Transformation
  ↓
AI Classification (if applicable)
  ↓
Conditional Routing
  ↓
External API Actions (Linear/Slack/Airtable)
  ↓
Success/Error Notifications
```

### Common Node Configurations

#### Slack Integration
- Bot user: n8n bot (ID: ouUfL3qTCI9EQhUE)
- Common channels: #bugs, #urgent-bugs, #clinician-bugs
- DM notifications to specific users

#### Linear Integration
- Team ID: `6b59e81d-57ac-4f81-af62-9986e383fc2e` (Engineering)
- Label categories: L0 (Team), L1 (Root Cause), L2 (Capability)
- GraphQL endpoint for advanced operations

#### Airtable Integration
- Base ID: `app0yJQw5vWqkK5ll` (Everlab FHIR)
- User Table: `tblWHMNMtlJX7tS9u`
- Practitioner Table: `tblMPnG6QvTKgJZF7`

## n8n MCP Tools Usage

### Available Tools
- `mcp__n8n-mcp__list_workflows` - List all workflows
- `mcp__n8n-mcp__get_workflow` - Get workflow details
- `mcp__n8n-mcp__create_workflow` - Create new workflow
- `mcp__n8n-mcp__update_workflow` - Update existing workflow
- `mcp__n8n-mcp__activate_workflow` - Activate workflow
- `mcp__n8n-mcp__deactivate_workflow` - Deactivate workflow
- `mcp__n8n-mcp__run_webhook` - Execute webhook workflow

### Workflow Export/Import Process
1. Export: Use `get_workflow` to retrieve JSON
2. Save to appropriate directory (active/inactive)
3. Sanitize filename (lowercase, hyphens)
4. Update metadata.json
5. Commit with descriptive message

## Best Practices

### Security
- Never store credentials in workflow JSON
- Use credential references only
- Sanitize webhook inputs
- Implement rate limiting where appropriate

### Error Handling
- Add error notification nodes
- Use try-catch in code nodes
- Log errors to appropriate channels
- Implement retry logic for transient failures

### Performance
- Batch API operations when possible
- Use webhook responses efficiently
- Implement caching for repeated lookups
- Monitor execution times

## Common Tasks

### Adding a New Bug Triaging Channel
1. Duplicate existing bug workflow
2. Update Slack trigger channel
3. Adjust classification rules if needed
4. Update notification destinations
5. Test with sample messages

### Updating AI Classification Logic
1. Locate the AI node (usually Anthropic Chat Model)
2. Update system prompt or categories
3. Test with variety of inputs
4. Monitor accuracy for few days
5. Adjust based on false positives/negatives

### Healthcare Workflow Considerations
- Ensure HIPAA compliance
- No PII in workflow definitions
- Audit logging for all operations
- Encrypted webhook payloads
- Regular credential rotation

## Troubleshooting

### Common Issues
1. **Webhook not triggering**: Check webhook URL and n8n instance status
2. **AI classification errors**: Review prompt clarity and examples
3. **API rate limits**: Implement backoff and queuing
4. **Missing credentials**: Verify in n8n UI, not in JSON

### Debug Steps
1. Check n8n execution logs
2. Review error outputs in failed executions
3. Test individual nodes in isolation
4. Verify external service availability
5. Check credential permissions

## Future Enhancements

Consider these patterns for new workflows:
- Implement workflow monitoring dashboard
- Add automated testing for critical paths
- Create reusable sub-workflows
- Implement blue-green deployment for workflows
- Add performance metrics collection

## Important Notes

1. **Production Changes**: Always test in dev first
2. **Backup**: Export before major modifications  
3. **Documentation**: Update README for new workflows
4. **Monitoring**: Set up alerts for workflow failures
5. **Compliance**: Healthcare workflows need extra review

Remember: This repository is the source of truth for all Everlab automation. Changes here directly impact business operations.
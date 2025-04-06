# Remove duplicates from YAML config file

```python
import yaml
import collections

file_name = 'x_duplicates.yaml'
titles = []


def load_titles():
    for option in chat_completion_options['chat_completions']:
        title = option['title']
        titles.append(title)
    return titles


def convert_content_fields(chat_completion_options):
    # Ensure YAML formatting consistency
    class LiteralString(str):
        pass

    def literal_representer(dumper, data):
        return dumper.represent_scalar('tag:yaml.org,2002:str', data, style='|')

    yaml.add_representer(LiteralString, literal_representer)

    # Convert `content` field to literal string (multiline format)
    for option in uniques:
        for message in option.get('messages', []):
            if 'content' in message:
                message['content'] = LiteralString(message['content'])

    yaml_data = {"chat_completions": uniques}

    return yaml_data


def show_duplicate_titles(titles):
    res = collections.Counter(titles)
    for k, v in res.items():
        if v > 1:
            print(k, v)


def show_number_of_duplicates(titles):
    print(len(titles))
    print(len(set(titles)))


def read_config_file(file_name):
    with open(file_name, 'r') as file:
        return yaml.safe_load(file)


def write_unique_data(file_name, unique_data):
    with open(file_name, 'w') as file:
        yaml.dump(unique_data, file, default_flow_style=False,
                  sort_keys=False, width=80, indent=2)


# Load data and identify duplicates
chat_completion_options = read_config_file(file_name)

titles = load_titles()
show_number_of_duplicates(titles)
show_duplicate_titles(titles)

# Remove duplicates while preserving format
uniques = []
for option in chat_completion_options['chat_completions']:
    if option not in uniques:
        uniques.append(option)

print(len(uniques))


file_name2 = 'x_no_duplicates.yaml'
yaml_data = convert_content_fields(chat_completion_options)
write_unique_data(file_name2, yaml_data)


print("Unique entries written to x_no_duplicates.yaml with preserved format")
```

The YAML file:

```yaml
chat_completions:

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Unit Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/unit-testing/index.html"
    title: Unit Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Integration Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/integration-testing/index.html"
    title: Integration Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term System Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/system-testing/index.html"
    title: System Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term End-to-End (E2E) Testing. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/e2e-testing/index.html"
    title: End-to-End (E2E) Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Smoke Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/smoke-testing/index.html"
    title: Smoke Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Sanity Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/sanity-testing/index.html"
    title: Sanity Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Regression Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/regression-testing/index.html"
    title: Regression Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Acceptance Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/acceptance-testing/index.html"
    title: Acceptance Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Alpha Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/alpha-testing/index.html"
    title: Alpha Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Beta Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/beta-testing/index.html"
    title: Beta Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term User Acceptance Testing (UAT). Provide in-depth definitions
          at the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/uat/index.html"
    title: User Acceptance Testing (UAT)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Contract Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/contract-testing/index.html"
    title: Contract Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Component Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/component-testing/index.html"
    title: Component Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Interface Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/interface-testing/index.html"
    title: Interface Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Backend Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/backend-testing/index.html"
    title: Backend Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Frontend Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/frontend-testing/index.html"
    title: Frontend Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Database Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/database-testing/index.html"
    title: Database Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term API Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/api-testing/index.html"
    title: API Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Web Service Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/web-service-testing/index.html"
    title: Web Service Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Mobile Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/mobile-testing/index.html"
    title: Mobile Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Cross-Browser Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/cross-browser-testing/index.html"
    title: Cross-Browser Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Cross-Platform Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/cross-platform-testing/index.html"
    title: Cross-Platform Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Internationalization Testing. Provide in-depth definitions
          at the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/internationalization-testing/index.html"
    title: Internationalization Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Localization Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/localization-testing/index.html"
    title: Localization Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Accessibility Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/accessibility-testing/index.html"
    title: Accessibility Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Compliance Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/compliance-testing/index.html"
    title: Compliance Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Certification Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/certification-testing/index.html"
    title: Certification Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Pilot Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/pilot-testing/index.html"
    title: Pilot Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Parallel Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/parallel-testing/index.html"
    title: Parallel Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Comparative Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/comparative-testing/index.html"
    title: Comparative Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Black Box Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/black-box-testing/index.html"
    title: Black Box Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term White Box Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/white-box-testing/index.html"
    title: White Box Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Gray Box Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/gray-box-testing/index.html"
    title: Gray Box Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Positive Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/positive-testing/index.html"
    title: Positive Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Negative Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/negative-testing/index.html"
    title: Negative Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Boundary Value Analysis. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/boundary-value-analysis/index.html"
    title: Boundary Value Analysis

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Equivalence Partitioning. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/equivalence-partitioning/index.html"
    title: Equivalence Partitioning

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Decision Table Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/decision-table-testing/index.html"
    title: Decision Table Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term State Transition Testing. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/state-transition-testing/index.html"
    title: State Transition Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Use Case Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/use-case-testing/index.html"
    title: Use Case Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Exploratory Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/exploratory-testing/index.html"
    title: Exploratory Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Ad Hoc Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/ad-hoc-testing/index.html"
    title: Ad Hoc Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Monkey Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/monkey-testing/index.html"
    title: Monkey Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Chaos Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/chaos-testing/index.html"
    title: Chaos Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Mutation Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/mutation-testing/index.html"
    title: Mutation Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Model-Based Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/model-based-testing/index.html"
    title: Model-Based Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Risk-Based Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/risk-based-testing/index.html"
    title: Risk-Based Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Scenario Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/scenario-testing/index.html"
    title: Scenario Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Pairwise Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/pairwise-testing/index.html"
    title: Pairwise Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Orthogonal Array Testing. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/orthogonal-array-testing/index.html"
    title: Orthogonal Array Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term All-Pairs Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the laikā
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/all-pairs-testing/index.html"
    title: All-Pairs Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Fuzz Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/fuzz-testing/index.html"
    title: Fuzz Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Property-Based Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/property-based-testing/index.html"
    title: Property-Based Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term A/B Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/ab-testing/index.html"
    title: A/B Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Canary Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/canary-testing/index.html"
    title: Canary Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Performance Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/performance-testing/index.html"
    title: Performance Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Load Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/load-testing/index.html"
    title: Load Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Stress Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/stress-testing/index.html"
    title: Stress Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Soak Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/soak-testing/index.html"
    title: Soak Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Spike Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/spike-testing/index.html"
    title: Spike Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Volume Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/volume-testing/index.html"
    title: Volume Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Scalability Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/scalability-testing/index.html"
    title: Scalability Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Reliability Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/reliability-testing/index.html"
    title: Reliability Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Stability Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/stability-testing/index.html"
    title: Stability Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Endurance Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/endurance-testing/index.html"
    title: Endurance Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Failover Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/failover-testing/index.html"
    title: Failover Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Recovery Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/recovery-testing/index.html"
    title: Recovery Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Security Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/security-testing/index.html"
    title: Security Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Penetration Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/penetration-testing/index.html"
    title: Penetration Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Vulnerability Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/vulnerability-testing/index.html"
    title: Vulnerability Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Ethical Hacking. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/ethical-hacking/index.html"
    title: Ethical Hacking

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term OWASP Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/owasp-testing/index.html"
    title: OWASP Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Compliance Testing (GDPR, HIPAA). Provide in-depth definitions
          at the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/compliance-testing-gdpr-hipaa/index.html"
    title: Compliance Testing (GDPR, HIPAA)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Usability Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/usability-testing/index.html"
    title: Usability Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term User Experience (UX) Testing. Provide in-depth definitions
          at the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/ux-testing/index.html"
    title: User Experience (UX) Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Compatibility Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/compatibility-testing/index.html"
    title: Compatibility Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Interoperability Testing. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/interoperability-testing/index.html"
    title: Interoperability Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Install/Uninstall Testing. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/install-uninstall-testing/index.html"
    title: Install/Uninstall Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Configuration Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/configuration-testing/index.html"
    title: Configuration Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Automation. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-automation/index.html"
    title: Test Automation

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Script. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-script/index.html"
    title: Test Script

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Harness. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-harness/index.html"
    title: Test Harness

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Framework. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-framework/index.html"
    title: Test Framework

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Runner. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-runner/index.html"
    title: Test Runner

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Double. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-double/index.html"
    title: Test Double

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Mock Object. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/mock-object/index.html"
    title: Mock Object

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Stub. Provide in-depth definitions at the start of the
          tutorial. Also put the term in broader context. Use p, h2, table,
          ul tags. Provide detailed explanations. Don't do other changes
          to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/stub/index.html"
    title: Stub

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Fake. Provide in-depth definitions at the start of the
          tutorial. Also put the term in broader context. Use p, h2, table,
          ul tags. Provide detailed explanations. Don't do other changes
          to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/fake/index.html"
    title: Fake

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Dummy. Provide in-depth definitions at the start of the
          tutorial. Also put the term in broader context. Use p, h2, table,
          ul tags. Provide detailed explanations. Don't do other changes
          to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/dummy/index.html"
    title: Dummy

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Fixture. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-fixture/index.html"
    title: Test Fixture

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Data. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-data/index.html"
    title: Test Data

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Data-Driven Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/data-driven-testing/index.html"
    title: Data-Driven Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Keyword-Driven Testing. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/keyword-driven-testing/index.html"
    title: Keyword-Driven Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Behavior-Driven Development (BDD). Provide in-depth
          definitions at the start of the tutorial. Also put the term in
          broader context. Use p, h2, table, ul tags. Provide detailed
          explanations. Don't do other changes to the document template
          and don't omit anything from the document template. In paragraphs,
          limit the sentences to 80 chars. Have max 5 sentences per paragraph.
          Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/bdd/index.html"
    title: Behavior-Driven Development (BDD)

  - messages:
      - roleRadius: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test-Driven Development (TDD). Provide in-depth definitions
          at the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/tdd/index.html"
    title: Test-Driven Development (TDD)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Page Object Model (POM). Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/pom/index.html"
    title: Page Object Model (POM)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Continuous Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/continuous-testing/index.html"
    title: Continuous Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Self-Healing Tests. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/self-healing-tests/index.html"
    title: Self-Healing Tests

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Visual Regression Testing. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/visual-regression-testing/index.html"
    title: Visual Regression Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Snapshot Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/snapshot-testing/index.html"
    title: Snapshot Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Static Application Security Testing (SAST). Provide in-depth
          definitions at the start of the tutorial. Also put the term in
          broader context. Use p, h2, table, ul tags. Provide detailed
          explanations. Don't do other changes to the document template
          and don't omit anything from the document template. In paragraphs,
          limit the sentences to 80 chars. Have max 5 sentences per paragraph.
          Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/sast/index.html"
    title: Static Application Security Testing (SAST)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Dynamic Application Security Testing (DAST). Provide in-depth
          definitions at the start of the tutorial. Also put the term in
          broader context. Use p, h2, table, ul tags. Provide detailed
          explanations. Don't do other changes to the document template
          and don't omit anything from the document template. In paragraphs,
          limit the sentences to 80 chars. Have max 5 sentences per paragraph.
          Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/dast/index.html"
    title: Dynamic Application Security Testing (DAST)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Interactive Testing (IAST). Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/iast/index.html"
    title: Interactive Testing (IAST)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term SQL Injection. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/sql-injection/index.html"
    title: SQL Injection

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term XSS (Cross-Site Scripting). Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/xss/index.html"
    title: XSS (Cross-Site Scripting)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term CSRF (Cross-Site Request Forgery). Provide in-depth
          definitions at the start of the tutorial. Also put the term in
          broader context. Use p, h2, table, ul tags. Provide detailed
          explanations. Don't do other changes to the document template
          and don't omit anything from the document template. In paragraphs,
          limit the sentences to 80 chars. Have max 5 sentences per paragraph.
          Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/csrf/index.html"
    title: CSRF (Cross-Site Request Forgery)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term DDoS Simulation. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/ddos-simulation/index.html"
    title: DDoS Simulation

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term OAuth Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/oauth-testing/index.html"
    title: OAuth Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term JWT Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/jwt-testing/index.html"
    title: JWT Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Secrets Scanning. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/secrets-scanning/index.html"
    title: Secrets Scanning

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Plan. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-plan/index.html"
    title: Test Plan

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Strategy. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-strategy/index.html"
    title: Test Strategy

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Case. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-case/index.html"
    title: Test Case

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Scenario. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-scenario/index.html"
    title: Test Scenario

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Suite. Provide weekly in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-suite/index.html"
    title: Test Suite

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Traceability Matrix. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/traceability-matrix/index.html"
    title: Traceability Matrix

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Coverage. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-coverage/index.html"
    title: Test Coverage

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Code Coverage. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/code-coverage/index.html"
    title: Code Coverage

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Requirements Coverage. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/requirements-coverage/index.html"
    title: Requirements Coverage

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Defect Lifecycle. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/defect-lifecycle/index.html"
    title: Defect Lifecycle

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Bug Triage. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/bug-triage/index.html"
    title: Bug Triage

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Root Cause Analysis. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/root-cause-analysis/index.html"
    title: Root Cause Analysis

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Metrics. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-metrics/index.html"
    title: Test Metrics

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Exit Criteria. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/exit-criteria/index.html"
    title: Exit Criteria

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Entry Criteria. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/entry-criteria/index.html"
    title: Entry Criteria

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Shift-Left Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/shift-left-testing/index.html"
    title: Shift-Left Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Shift-Right Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/shift-right-testing/index.html"
    title: Shift-Right Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Blue-Green Deployment Testing. Provide in-depth definitions
          at the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/blue-green-deployment-testing/index.html"
    title: Blue-Green Deployment Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Feature Flag Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/feature-flag-testing/index.html"
    title: Feature Flag Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Infrastructure Testing. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/infrastructure-testing/index.html"
    title: Infrastructure Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Immutable Infrastructure Testing. Provide in-depth
          definitions at the start of the tutorial. Also put the term in
          broader context. Use p, h2, table, ul tags. Provide detailed
          explanations. Don't do other changes to the document template
          and don't omit anything from the document template. In paragraphs,
          limit the sentences to 80 chars. Have max 5 sentences per paragraph.
          Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/immutable-infrastructure-testing/index.html"
    title: Immutable Infrastructure Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term GitOps Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/gitops-testing/index.html"
    title: GitOps Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Pipeline Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/pipeline-testing/index.html"
    title: Pipeline Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Deployment Verification. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/deployment-verification/index.html"
    title: Deployment Verification

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Rollback Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/rollback-testing/index.html"
    title: Rollback Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term AI-Based Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/ai-based-testing/index.html"
    title: AI-Based Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Self-Healing Tests. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/self-healing-tests/index.html"
    title: Self-Healing Tests

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term No-Code Test Automation. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/no-code-test-automation/index.html"
    title: No-Code Test Automation

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Synthetic Monitoring. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/synthetic-monitoring/index.html"
    title: Synthetic Monitoring

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Observability-Driven Testing. Provide in-depth definitions
          at the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/observability-driven-testing/index.html"
    title: Observability-Driven Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Digital Twin Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/digital-twin-testing/index.html"
    title: Digital Twin Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Metaverse Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/metaverse-testing/index.html"
    title: Metaverse Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Quantum Computing Testing. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/quantum-computing-testing/index.html"
    title: Quantum Computing Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term 5G Network Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/5g-network-testing/index.html"
    title: 5G Network Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Chaos Engineering. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/chaos-engineering/index.html"
    title: Chaos Engineering

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term IEEE 829 (Test Documentation Standard). Provide in-depth
          definitions at the start of the tutorial. Also put the term in
          broader context. Use p, h2, table, ul tags. Provide detailed
          explanations. Don't do other changes to the document template
          and don't omit anything from the document template. In paragraphs,
          limit the sentences to 80 chars. Have max 5 sentences per paragraph.
          Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/ieee-829/index.html"
    title: IEEE 829 (Test Documentation Standard)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term ISO 25010 (Quality Model). Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/iso-25010/index.html"
    title: ISO 25010 (Quality Model)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term ISTQB Glossary. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/istqb-glossary/index.html"
    title: ISTQB Glossary

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Charter (Exploratory Testing). Provide in-depth
          definitions at the start of the tutorial. Also put the term in
          broader context. Use p, h2, table, ul tags. Provide detailed
          explanations. Don't do other changes to the document template
          and don't omit anything from the document template. In paragraphs,
          limit the sentences to 80 chars. Have max 5 sentences per paragraph.
          Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-charter/index.html"
    title: Test Charter (Exploratory Testing)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Oracle. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-oracle/index.html"
    title: Test Oracle

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Heuristic Test Strategy Model. Provide in-depth definitions
          at the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/heuristic-test-strategy-model/index.html"
    title: Heuristic Test Strategy Model

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Quality Gate. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/quality-gate/index.html"
    title: Quality Gate

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Policy. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-policy/index.html"
    title: Test Policy

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Summary Report. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-summary-report/index.html"
    title: Test Summary Report

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Defect Report. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/defect-report/index.html"
    title: Defect Report

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Lead. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-lead/index.html"
    title: Test Lead

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Engineer. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-engineer/index.html"
    title: Test Engineer

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term SDET (Software Development Engineer in Test). Provide
          in-depth definitions at the start of the tutorial. Also put the
          term in broader context. Use p, h2, table, ul tags. Provide
          detailed explanations. Don't do other changes to the document
          template and don't omit anything from the document template. In
          paragraphs, limit the sentences to 80 chars. Have max 5 sentences
          per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/sdet/index.html"
    title: SDET (Software Development Engineer in Test)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Quality Analyst. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/quality-analyst/index.html"
    title: Quality Analyst

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Data Manager. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-data-manager/index.html"
    title: Test Data Manager

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Environment Manager. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-environment-manager/index.html"
    title: Test Environment Manager

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Log. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-log/index.html"
    title: Test Log

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Execution Report. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-execution-report/index.html"
    title: Test Execution Report

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Closure Report. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-closure-report/index.html"
    title: Test Closure Report

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Lead. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-lead/index.html"
    title: Test Lead

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Engineer. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-engineer/index.html"
    title: Test Engineer

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term SDET (Software Development Engineer in Test). Provide
          in-depth definitions at the start of the tutorial. Also put the
          term in broader context. Use p, h2, table, ul tags. Provide
          detailed explanations. Don't do other changes to the document
          template and don't omit anything from the document template. In
          paragraphs, limit the sentences to 80 chars. Have max 5 sentences
          per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/sdet/index.html"
    title: SDET (Software Development Engineer in Test)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Quality Analyst. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/quality-analyst/index.html"
    title: Quality Analyst

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Data Manager. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-data-manager/index.html"
    title: Test Data Manager

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Environment Manager. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-environment-manager/index.html"
    title: Test Environment Manager

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Log. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-log/index.html"
    title: Test Log

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Execution Report. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-execution-report/index.html"
    title: Test Execution Report

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Closure Report. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-closure-report/index.html"
    title: Test Closure Report

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term AI-Based Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/ai-based-testing/index.html"
    title: AI-Based Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Self-Healing Tests. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/self-healing-tests/index.html"
    title: Self-Healing Tests

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term No-Code Test Automation. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/no-code-test-automation/index.html"
    title: No-Code Test Automation

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Synthetic Monitoring. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/synthetic-monitoring/index.html"
    title: Synthetic Monitoring

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Observability-Driven Testing. Provide in-depth definitions
          at the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/observability-driven-testing/index.html"
    title: Observability-Driven Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Digital Twin Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/digital-twin-testing/index.html"
    title: Digital Twin Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Metaverse Testing. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/metaverse-testing/index.html"
    title: Metaverse Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Quantum Computing Testing. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/quantum-computing-testing/index.html"
    title: Quantum Computing Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term 5G Network Testing. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/5g-network-testing/index.html"
    title: 5G Network Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Chaos Engineering. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/chaos-engineering/index.html"
    title: Chaos Engineering

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term IEEE 829 (Test Documentation Standard). Provide in-depth
          definitions at the start of the tutorial. Also put the term in
          broader context. Use p, h2, table, ul tags. Provide detailed
          explanations. Don't do other changes to the document template
          and don't omit anything from the document template. In paragraphs,
          limit the sentences to 80 chars. Have max 5 sentences per paragraph.
          Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/ieee-829/index.html"
    title: IEEE 829 (Test Documentation Standard)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term ISO 25010 (Quality Model). Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/iso-25010/index.html"
    title: ISO 25010 (Quality Model)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term ISTQB Glossary. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/istqb-glossary/index.html"
    title: ISTQB Glossary

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Charter (Exploratory Testing). Provide in-depth
          definitions at the start of the tutorial. Also put the term in
          broader context. Use p, h2, table, ul tags. Provide detailed
          explanations. Don't do other changes to the document template
          and don't omit anything from the document template. In paragraphs,
          limit the sentences to 80 chars. Have max 5 sentences per paragraph.
          Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-charter/index.html"
    title: Test Charter (Exploratory Testing)

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Oracle. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-oracle/index.html"
    title: Test Oracle      

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Heuristic Test Strategy Model. Provide in-depth definitions
          at the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/heuristic-test-strategy-model/index.html"
    title: Heuristic Test Strategy Model

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Quality Gate. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/quality-gate/index.html"
    title: Quality Gate

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Policy. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-policy/index.html"
    title: Test Policy

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Summary Report. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-summary-report/index.html"
    title: Test Summary Report

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Defect Report. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/defect-report/index.html"
    title: Defect Report

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Risk Assessment in Testing. Provide in-depth definitions at
          the start of the tutorial. Also put the term in broader context.
          Use p, h2, table, ul tags. Provide detailed explanations. Don't
          do other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/risk-assessment-in-testing/index.html"
    title: Risk Assessment in Testing

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Estimation. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-estimation/index.html"
    title: Test Estimation

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Governance. Provide in-depth definitions at the start
          of the tutorial. Also put the term in broader context. Use p, h2,
          table, ul tags. Provide detailed explanations. Don't do other
          changes to the document template and don't omit anything from the
          document template. In paragraphs, limit the sentences to 80 chars.
          Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-governance/index.html"
    title: Test Governance

  - messages:
      - role: "user"
        content: |
          Write an in-depth, detailed and complete article covering the
          term Test Maturity Model. Provide in-depth definitions at the
          start of the tutorial. Also put the term in broader context. Use
          p, h2, table, ul tags. Provide detailed explanations. Don't do
          other changes to the document template and don't omit anything
          from the document template. In paragraphs, limit the sentences to
          80 chars. Have max 5 sentences per paragraph. Output in HTML.
    temperature: 0.7
    top_p: 0.9
    model: "deepseek-chat"
    max_completion_tokens: 90000
    path: "data/testing/test-maturity-model/index.html"
    title: Test Maturity Model
```

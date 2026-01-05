# Hassania Data Pipeline

A data collection and processing pipeline for authentic Hassaniya dialect content, designed for fine-tuning language models.

## Overview

This repository contains:
- **Raw data** collected from various Mauritanian sources
- **Processed data** in HDRP (Hassaniya Dialect Resource Package) format
- **Conversion scripts** to transform raw data into training-ready formats

## Dataset Statistics

| Bucket | Episodes |
|--------|----------|
| everyday_chat | 229 |
| marketplace_qa | 100 |
| public_comments | 4 |
| **Total** | **333** |

### Data Sources

| Source | Episodes | Description |
|--------|----------|-------------|
| Peace Corps Mauritania | 138 | Official language learning materials |
| YouTube Language Beat | 51 | Video lesson transcriptions |
| Voursa Marketplace | 56 | Mauritanian marketplace listings |
| Facebook Marketplace Nouakchott | 44 | Local marketplace content |
| mo3jam Dictionary | 26 | User-contributed vocabulary |
| Omniglot | 18 | Linguistic reference materials |

## Data Format

### HDRP Format (JSONL)

Each line contains a JSON object with:

```json
{
  "english": "English translation or description",
  "hassaniya_ar": "الحسانية بالعربية",
  "hassaniya_en": "Hassaniya in Latin transliteration",
  "bucket": "everyday_chat|marketplace_qa|public_comments",
  "source": "data_source_identifier",
  "context": "situational_context"
}
```

### Buckets

- **everyday_chat**: Greetings, phrases, vocabulary for daily conversation
- **marketplace_qa**: Product descriptions, real estate listings, marketplace vocabulary
- **public_comments**: Social media posts, proverbs, public discussions

## Directory Structure

```
hassania-data-pipeline/
├── data/
│   ├── raw/                    # Original collected data
│   │   ├── dictionary/         # Dictionary sources
│   │   ├── facebook/           # Facebook page data
│   │   ├── marketplace/        # Voursa and FB Marketplace
│   │   ├── reference/          # Peace Corps, Omniglot
│   │   ├── social/             # Social media content
│   │   └── video/              # YouTube lesson transcriptions
│   └── processed/              # HDRP format output
│       ├── hassaniya_hdrp.csv
│       ├── hassaniya_hdrp.jsonl
│       └── hdrp_summary.json
├── scripts/
│   └── convert_to_hdrp.py      # Data conversion script
└── README.md
```

## Usage

### Converting Raw Data to HDRP Format

```bash
python3 scripts/convert_to_hdrp.py
```

### Loading the Dataset

```python
import json

# Load JSONL format
episodes = []
with open('data/processed/hassaniya_hdrp.jsonl', 'r') as f:
    for line in f:
        episodes.append(json.loads(line))

# Filter by bucket
everyday_chat = [e for e in episodes if e['bucket'] == 'everyday_chat']
```

## Hassaniya Dialect Notes

Hassaniya Arabic is spoken by approximately 9.5 million people across:
- Mauritania (3.5 million)
- Algeria (4.8 million, mainly Tindouf)
- Western Sahara (424,000)
- Morocco, Senegal, Mali

### Key Features

- Heavy French loanwords (نيمرو, ابرتماه, كوزين)
- Unique vocabulary (احذ = near, اكريب = close, ذوك = these)
- Distinct phonology with sounds like /g/ (گ) and /v/ (ڤ)

## License

Data is collected from publicly available sources for research and educational purposes.

## Contributing

To add more Hassaniya data:
1. Add raw data files to `data/raw/` in JSON format
2. Update `scripts/convert_to_hdrp.py` to process new sources
3. Run the conversion script
4. Submit a pull request

## References

- [Peace Corps Mauritania Language Materials](https://files.peacecorps.gov/)
- [Omniglot Hassaniya Arabic](https://www.omniglot.com/writing/arabic_hassaniya.htm)
- [DAH Dataset on Hugging Face](https://huggingface.co/datasets/hassan-IA/dah)
- [mo3jam Hassaniya Dictionary](https://ar.mo3jam.com/dialect/Hassaniya)

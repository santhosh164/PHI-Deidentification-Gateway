# Known Failures and Limitations

## 1. Dataset Limitations
- Training data consists of 403 synthetic examples.
- Independent evaluation uses only 20 synthetic clinical notes.
- More diverse clinical text is required for reliable generalization.

## 2. Entity Detection
- Some location names may be incorrectly detected or have incorrect boundaries.
- Unseen names and identifier formats may be missed.
- Some PHI categories have limited evaluation examples.

## 3. Age Detection
- Ages above 89 are intended to be masked.
- Ages 89 and below should remain visible.
- Different age expressions need further testing.

## 4. Long Documents
- The current model processes a maximum of 512 tokens per input.
- Longer clinical notes may be truncated.

## 5. LLM Integration
- External LLM integration and end-to-end restoration are not yet validated.
- Modified or unknown placeholders may not be restored correctly.

## 6. Evaluation Limitations
- Evaluation results are based on a small synthetic test set.
- The custom model and Presidio results currently use different evaluation methods.
- A common exact-span evaluation is needed for a fair comparison.

## 7. Security and Deployment
- The mapping dictionary must remain local and must never be sent to an external LLM.
- The prototype has not been validated for real patient data.
- It is not certified as HIPAA-compliant or ready for clinical deployment.

## Future Improvements
- Increase synthetic training and test data.
- Improve name and location detection.
- Support long clinical documents.
- Complete LLM integration and utility testing.
- Perform broader privacy and security validation.

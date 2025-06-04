# Chainguard Libraries Demo: Preventing Distribution Supply Chain Attacks in PyPI

## Ultralytics Supply Chain Attack

### Background Information

This code was injected into the PyPI release artifacts and was not present in the public GitHub repository.  
See the advisory details here:
- [PYSEC-2024-154](https://github.com/pypa/advisory-database/blob/main/vulns/ultralytics/PYSEC-2024-154.yaml#L12-L13)
- [Ultralytics GitHub Pull Request](https://github.com/ultralytics/ultralytics/pull/18020?ref=blog.gitguardian.com#issuecomment-2525180194)

#### Pull Request introduced the blobs (that never got merged and were later force deleted)
- [GitHub Issue Comment 1](https://github.com/ultralytics/ultralytics/issues/18027#issuecomment-2526084417)
- [GitHub Issue Comment 2](https://github.com/ultralytics/ultralytics/issues/18027#issuecomment-2520462686)

#### Triggering Commit for Malicious Release
The commit `Cb260c243ffa3e0cc84820095cd88be2f5db86ca` is the triggering commit for the first malicious release.  
It bumps the version to the first known malicious version (`v8.3.41`) and critically removes a `github.actor` check that limited who could do `publish.yml` triggers from the main branch.  
This commit is authored by the `@UltralyticsAssistant` which strongly suggests that the attacker is in full control of the `@UltralyticsAssistant` identity at this point.  
- [GitHub Commit](https://github.com/ultralytics/ultralytics/commit/cb260c243ffa3e0cc84820095cd88be2f5db86ca)
- [GitHub Action Run](https://github.com/ultralytics/ultralytics/actions/runs/12168072999/job/33938058724)

#### Sigstore Attestation attestations for malicious distributions of v8.3.41 on PyPI:
- ultralytics-8.3.41.tar.gz https://search.sigstore.dev/?logIndex=153415338
- ultralytics-8.3.41-py3-none-any.whl https://search.sigstore.dev/?logIndex=153415340
- The release `v8.3.41` was uploaded to PyPI with a Trusted Publisher and valid attestations for each distribution, matching the `ultralytics/ultralytics` Trusted Publisher identity.

#### Branch References and Removal of Malicious Payload Files
Each branch name referenced a file in a commit that supposedly contained a malicious payload used during the exploitation. However, both those files were removed from GitHub following the incident.

#### Attack Payload
As forecasted in Woodruff’s analysis, the actual attack payload is a copy of the proof of concept from Adnan Khan's post on GitHub Actions cache poisoning.  
It uses the same Python memory dump tool and HTTP data exfiltration channel. The exfiltration occurred to a temporary HTTP webhook:  
`hxxps://webhook.site/9212d4ee-df58-41db-886a-98d180a912e6` (which has been deleted since then).  
No other mention of this webhook was observed in our GitHub dataset.

#### Additional Resources:
- [Yossarian’s Blog on the Attack](https://blog.yossarian.net/2024/12/06/zizmor-ultralytics-injection?ref=blog.gitguardian.com)
- [GitHub Issue Comment](https://github.com/ultralytics/ultralytics/issues/18027#issuecomment-2520085978)
- [GitGuardian Blog on the Attack](https://blog.gitguardian.com/the-ultralytics-supply-chain-attack-connecting-the-dots-with-gitguardians-public-monitoring-data/)
- [Woodruff’s Analysis Gist](https://gist.github.com/woodruffw/7d6a07077842508b85008e0267f7f3bb)

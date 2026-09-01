# Commit subject prefixes

This repository is a fork of the Android Common Kernel (ACK) branch
`android14-6.1`. It follows `linux-stable` 6.1.y on its own, merging releases ACK
has not taken yet, and absorbs ACK's own updates as they land. Where
`linux-stable` and ACK disagree, ACK wins.

Everything about the tree follows ACK. The one thing this file exists to explain
is the extra subject prefix, because that is the one place where reading this
history differs from reading ACK's.

## The prefixes

A prefix says **where a change came from**, not who wrote it. The first six are
ACK's own convention, documented by Google; they appear here because they arrived
with ACK or with a `linux-stable` merge.

| Prefix | Origin |
| --- | --- |
| *(no prefix)* | A `linux-stable` commit, exactly as released. Arrives through a `Merge tag 'v6.1.N'` commit. |
| `UPSTREAM:` | Cherry-picked from mainline Linux without modification. |
| `BACKPORT:` | From mainline, modified to apply to this kernel. |
| `FROMGIT:` | From a maintainer tree, not yet in Linus' tree. |
| `FROMLIST:` | From a mailing list posting, not yet merged anywhere. |
| `ANDROID:` | Specific to the Android Common Kernel, no mainline equivalent. **Every `ANDROID:` commit in this repository came from ACK.** |
| `LUMINAIRE:` | Written for this fork. Not reviewed by Google, not in ACK, not in mainline. |

`ANDROID:` and `LUMINAIRE:` are the pair worth keeping straight. Strictly,
`ANDROID:` marks a change as Android-specific rather than as Google's work — but
inside an ACK branch's history every `ANDROID:` commit did come from ACK, so a
fork-authored commit carrying that prefix would be indistinguishable from one
Google reviewed. `LUMINAIRE:` removes the ambiguity.

To read every fork-authored change:

```
git log --first-parent --no-merges --grep='^LUMINAIRE:' android14-6.1-luminaire
```

A `LUMINAIRE:` subject may name the release that made the change necessary:

```
LUMINAIRE: fixup v6.1.183: keep Qdisc and ppp_channel KMI-stable
```

Merge commits take no prefix — a merge is an operation, not an authored patch.

Were such a change ever submitted to ACK, it would be reposted there as
`ANDROID:`. The prefix describes a change's home, and its home would have changed.

## Branches

| Branch | Contents |
| --- | --- |
| `android14-6.1` | Pure ACK, unmodified. Only ever fast-forwarded from Google. |
| `android14-6.1-luminaire` | ACK plus merged `linux-stable`. |
| `android14-6.1-staging` | The same, plus work in progress. **May not boot.** |

`android14-6.1-luminaire` always points at a commit that also exists in
`android14-6.1-staging`, and advances by fast-forward rather than by a separate
merge. So `staging` is normally ahead, and the gap is work in progress.

History here is occasionally rewritten — to correct a subject, or to move
`android14-6.1-luminaire` back to a known-good commit. When that happens
`git pull` will fail or make a mess. Recover with:

```
git fetch origin
git reset --hard origin/android14-6.1-luminaire
```

If you keep your own work on top, rebase it rather than merging.

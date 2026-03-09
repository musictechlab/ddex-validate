# ERN Message Structure Reference

## Root Element

`<ern:NewReleaseMessage>` — The top-level container for all ERN messages.

Required attributes:
- `MessageSchemaVersionId` — e.g., `ern/382`
- `LanguageAndScriptCode` — e.g., `en`
- `xmlns:ern` — Namespace URI: `http://ddex.net/xml/ern/{VERSION}`
- `xmlns:xs` — `http://www.w3.org/2001/XMLSchema-instance`
- `xs:schemaLocation` — Points to the official XSD

## Element Hierarchy

```
NewReleaseMessage
├── MessageHeader (required)
│   ├── MessageThreadId (required)
│   ├── MessageId (required)
│   ├── MessageSender (required)
│   │   ├── PartyId
│   │   └── PartyName > FullName
│   ├── SentOnBehalfOf (optional)
│   │   ├── PartyId
│   │   └── PartyName > FullName
│   ├── MessageRecipient (required, multiple allowed)
│   │   ├── PartyId
│   │   └── PartyName > FullName
│   ├── MessageCreatedDateTime (required, ISO 8601)
│   └── MessageControlType (required: LiveMessage | TestMessage)
│
├── ResourceList (required)
│   ├── SoundRecording (one or more)
│   │   ├── SoundRecordingType (MusicalWorkSoundRecording)
│   │   ├── SoundRecordingId
│   │   │   ├── ISRC (required, 12 alphanumeric chars)
│   │   │   ├── CatalogNumber (optional, with Namespace attr)
│   │   │   └── ProprietaryId (optional, with Namespace attr)
│   │   ├── ResourceReference (e.g., "A1" — used to link to releases)
│   │   ├── ReferenceTitle > TitleText
│   │   ├── Duration (ISO 8601 duration: PT3M30S)
│   │   └── SoundRecordingDetailsByTerritory
│   │       ├── TerritoryCode (ISO 3166-1 alpha-2 or "Worldwide")
│   │       ├── Title TitleType="DisplayTitle" > TitleText
│   │       ├── DisplayArtist
│   │       │   ├── PartyName > FullName
│   │       │   ├── PartyId (optional)
│   │       │   └── ArtistRole (MainArtist, FeaturedArtist, etc.)
│   │       ├── IndirectResourceContributor (optional, repeatable)
│   │       │   ├── PartyName > FullName
│   │       │   ├── PartyId (optional)
│   │       │   └── IndirectResourceContributorRole (Composer, Lyricist, etc.)
│   │       ├── RightsController (optional)
│   │       │   ├── PartyName > FullName
│   │       │   ├── PartyId
│   │       │   ├── RightsControllerRole
│   │       │   └── RightSharePercentage (0-100.00)
│   │       ├── OriginalResourceReleaseDate (YYYY-MM-DD)
│   │       ├── PLine > Year + PLineText
│   │       ├── Genre > GenreText + SubGenre (optional)
│   │       ├── ParentalWarningType (NotExplicit | Explicit | Unknown)
│   │       └── TechnicalSoundRecordingDetails
│   │           ├── TechnicalResourceDetailsReference (e.g., "T1")
│   │           ├── AudioCodecType (FLAC | WAV | MP3 | AAC)
│   │           ├── BitRate (bits per second, e.g., 320000)
│   │           ├── SamplingRate (Hz, e.g., 44100)
│   │           ├── IsPreview (true | false)
│   │           └── File
│   │               ├── FileName
│   │               ├── FilePath
│   │               └── HashSum > HashSum + HashSumAlgorithmType (MD5 | SHA1)
│   │
│   └── Image (optional, repeatable)
│       ├── ImageType (FrontCoverImage | BackCoverImage)
│       ├── ImageId > ProprietaryId
│       ├── ResourceReference (e.g., "A2")
│       └── ImageDetailsByTerritory
│           ├── TerritoryCode
│           └── TechnicalImageDetails
│               ├── TechnicalResourceDetailsReference
│               ├── ImageCodecType (JPEG | PNG | TIFF)
│               ├── ImageHeight (pixels, min 1500 for most DSPs)
│               ├── ImageWidth (pixels, should match height for square art)
│               └── File > FileName + FilePath + HashSum
│
├── ReleaseList (required)
│   └── Release (one or more)
│       ├── ReleaseId
│       │   ├── ICPN (UPC/EAN: 12-13 digits)
│       │   ├── CatalogNumber (optional)
│       │   └── ProprietaryId (optional)
│       ├── ReleaseReference (e.g., "R0")
│       ├── ReferenceTitle > TitleText
│       ├── ReleaseResourceReferenceList
│       │   └── ReleaseResourceReference (must match ResourceReference values)
│       ├── ReleaseType (Album | Single | EP | Bundle | etc.)
│       ├── ReleaseDetailsByTerritory
│       │   ├── TerritoryCode
│       │   ├── DisplayArtistName (string)
│       │   ├── LabelName (string)
│       │   ├── Title TitleType="FormalTitle" > TitleText
│       │   ├── Title TitleType="DisplayTitle" > TitleText
│       │   ├── DisplayArtist (same structure as in SoundRecording)
│       │   ├── ParentalWarningType
│       │   ├── ResourceGroup (defines track ordering)
│       │   │   └── ResourceGroup (nested)
│       │   │       ├── Title TitleType="GroupingTitle" > TitleText
│       │   │       ├── SequenceNumber
│       │   │       └── ResourceGroupContentItem
│       │   │           ├── SequenceNumber
│       │   │           ├── ResourceType (SoundRecording | Image)
│       │   │           └── ReleaseResourceReference ReleaseResourceType="PrimaryResource"
│       │   ├── Genre > GenreText + SubGenre
│       │   └── OriginalReleaseDate (YYYY-MM-DD)
│       ├── PLine > Year + PLineText
│       └── CLine > Year + CLineText
│
└── DealList (optional but expected by DSPs)
    └── ReleaseDeal
        ├── DealReleaseReference (links to ReleaseReference)
        └── Deal
            └── DealTerms
                ├── CommercialModelType (SubscriptionModel | PayAsYouGoModel | etc.)
                ├── Usage > UseType (OnDemandStream | PermanentDownload | etc.)
                ├── TerritoryCode
                └── ValidityPeriod > StartDate
```

## Field Formats

| Field | Format | Example | Notes |
|-------|--------|---------|-------|
| ISRC | 12 alphanumeric | `USSM12345678` | No hyphens in XML |
| ICPN (UPC) | 12-13 digits | `123456789012` | Check digit included |
| EAN | 13 digits | `1234567890123` | European variant |
| PartyId | PADPIDA + id | `PADPIDA2023001` | DDEX Party Identifier |
| Duration | ISO 8601 | `PT3M30S` | 3 minutes 30 seconds |
| Date | ISO 8601 | `2024-09-10` | YYYY-MM-DD |
| DateTime | ISO 8601 | `2024-09-10T12:00:00` | With time |
| TerritoryCode | ISO 3166-1 | `US`, `GB`, `Worldwide` | Alpha-2 or "Worldwide" |
| RightSharePercentage | Decimal | `100.00` | 0 to 100 |
| SamplingRate | Integer (Hz) | `44100` | CD quality |
| BitRate | Integer (bps) | `320000` | 320 kbps |

## ERN Version Differences

### ERN 3.8.2 (Legacy — still widely used)
- Namespace: `http://ddex.net/xml/ern/382`
- XSD: `https://service.ddex.net/xml/ern/382/release-notification.xsd`
- `DisplayArtistName` is a simple string in ReleaseDetailsByTerritory
- Broadly supported by all major distributors

### ERN 4.1.1+ (Current standard)
- Namespace: `http://ddex.net/xml/ern/411`
- XSD: `https://service.ddex.net/xml/ern/411/release-notification.xsd`
- Updated element naming conventions
- More granular territory and deal handling
- Enhanced contributor role vocabulary
- Some distributors still require 3.8.2

## DDEX Genre Vocabulary (Common)

Pop, Rock, Hip-Hop/Rap, R&B/Soul, Electronic, Dance, Jazz, Classical, Country, Folk, Latin, World, Reggae, Blues, Metal, Punk, Alternative, Indie, Ambient, Soundtrack, Children's Music, Spoken Word, Comedy, Holiday

## DSP-Specific Requirements

Most Digital Service Providers enforce additional requirements beyond the ERN schema:

- **Cover art**: Minimum 1500x1500 pixels, square, JPEG or PNG
- **Audio**: Lossless preferred (FLAC/WAV), minimum 16-bit/44.1kHz
- **DealList**: Required by Spotify, Apple Music, Amazon Music
- **ParentalWarningType**: Required — `Unknown` is accepted but not preferred
- **Genre**: Must match DSP's genre taxonomy (varies by platform)
- **Release date**: Must be in the future for new releases (pre-order window varies)

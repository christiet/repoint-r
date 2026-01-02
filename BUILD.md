# Building Repoint-R

Repoint-R is an Eclipse RCP application for Documentum repository interrogation, built with Maven Tycho.

## Prerequisites

- **Java 17** (or later)
- **Maven 3.9+** (Tycho 4.x requires Maven 3.9 or higher)

Verify your environment:

```bash
java -version    # Should show Java 17+
mvn --version    # Should show Maven 3.9+
```

## Build

From the project root:

```bash
mvn clean package
```

The build produces platform-specific distributions in:

| Platform | Output Location |
|----------|-----------------|
| Windows  | `repoint-eclipse-repository/target/products/Repoint-win32.win32.x86_64.zip` |
| Linux    | `repoint-eclipse-repository/target/products/Repoint-linux.gtk.x86_64.tar.gz` |

## Run

### 1. Extract the Archive

The archives extract their contents directly into the current directory (no wrapper folder).

**Windows:**
```bash
mkdir repoint && cd repoint
unzip ../Repoint-win32.win32.x86_64.zip
```

**Linux:**
```bash
mkdir repoint && cd repoint
tar xzf ../Repoint-linux.gtk.x86_64.tar.gz
```

After extraction, you should see:
```
repoint/
  repoint           # executable (Linux) or repoint.exe (Windows)
  repoint.ini
  plugins/
  features/
  configuration/
  ...
```

### 2. Install DFC JARs

DFC (Documentum Foundation Classes) JARs must be provided from your Documentum installation.

Find the DFC plugin directory - it includes a build timestamp in the version:
```bash
ls plugins/ | grep com.documentum.dfc
# Example: com.documentum.dfc_0.1.0.202601021339
```

Copy the following JARs into `plugins/com.documentum.dfc_<version>/lib/`:

| JAR | Description |
|-----|-------------|
| `dfc.jar` | Core DFC library |
| `ci.jar` | Content Intelligence |
| `DmcRecords.jar` | Records management |
| `xtrim-server.jar` | xTrim server |
| `xtrim-api.jar` | xTrim API |
| `jcifs-krb5-1.3.1.jar` | JCIFS Kerberos |
| `dms-client-api.jar` | DMS client API |
| `configservice-impl.jar` | Config service implementation |
| `configservice-api.jar` | Config service API |
| `log4j-api-2.19.0.jar` | Log4j API |
| `log4j-core-2.19.0.jar` | Log4j Core |
| `log4j-1.2-api-2.19.0.jar` | Log4j 1.2 bridge |
| `krbutil.jar` | Kerberos utilities |
| `aspectjrt.jar` | AspectJ runtime |
| `All-MB.jar` | MacBinary support |
| `commons-lang3-3.12.0.jar` | Apache Commons Lang |
| `commons-codec-1.15.jar` | Apache Commons Codec |
| `messageArchive.jar` | Message archive |
| `messageService.jar` | Message service |
| `collaboration.jar` | Collaboration support |
| `workflow.jar` | Workflow support |
| `subscription.jar` | Subscription support |
| `bpmutil.jar` | BPM utilities |
| `json.jar` | JSON support |
| `servlet-api.jar` | Servlet API |

These JARs are typically found in the `dfc` directory of a Documentum client or Content Server installation.

**Note:** If the `lib/` directory already contains JARs, they were included from the build environment. Verify they match your DFC version.

### 3. Configure dfc.properties

Ensure `dfc.properties` is configured with your docbroker connection details. Place it in a location accessible to the application (e.g., alongside the executable or in your classpath).

Alternatively, Repoint-R supports adding external docbrokers via the login window without modifying `dfc.properties`.

### 4. Launch the Application

**Windows:**
```bash
cd repoint
repoint.exe
```

**Linux:**
```bash
cd repoint
./repoint
```

## Known Issues

- **Java 9+ Module Conflicts**: Some DFC JARs bundle XML APIs that conflict with the JDK's module system. The following JARs have been disabled in the bundle classpath and should NOT be copied:
  - `xml-apis.jar`
  - `jaxb-api.jar`, `jaxb-impl.jar`, `jaxb-xjc.jar`
  - `jsr173_api.jar`
  - `activation.jar`

  If these exist in the lib directory with `.disabled` suffix, leave them as-is.

- **First Launch**: The `-clean` flag is passed by default to ensure OSGi bundles are properly initialised.

- **GTK Warnings (Linux)**: You may see GTK accessibility warnings on Linux - these are harmless and can be ignored.

## Project History

This fork modernises the original Repoint application:

- Upgraded from Eclipse 2020-06 to **Eclipse 2023-06** target platform
- Upgraded from Tycho 2.7.5 to **Tycho 4.0.4**
- Updated to **Java 17** runtime requirement
- Replaced deprecated `TableTree` widgets with `Tree`/`TreeColumn`
- Removed bundled XML JARs that conflict with Java 9+ module system

See issues #72, #75, and #76 in the Orchestra tracker for upgrade details.

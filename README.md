# Fedora 43 Java Change Proposal

## Glossary

- **Java** – A programming language and platform.

- **OpenJDK** – An open-source implementation of Java, typically packaged as `java-$x-openjdk`.

- **LTS OpenJDK** – Long-Term Support versions of OpenJDK, maintained upstream for extended periods. Packaged as `java-$version-openjdk`.

- **STS OpenJDK** – Short-Term Support versions of OpenJDK, maintained for around 6 months until the next major release. Packaged as `java-latest-openjdk`.

- **Java package** – A Fedora RPM package containing Java code compiled from source.

- **Java application** – A Java package designed for end users. It includes a launcher in `/usr/bin`.

- **Java library** – A Java package that is not a standalone application. It provides only JAR files and is intended to be used as a dependency in other Java applications.

- **Java SIG** – An informal group of contributors interested in Java in Fedora. We don't hold formal meetings or use a dedicated Matrix room, but we actively collaborate to deliver a great Java experience in Fedora.


## Compatibility

Java is renowned for its exceptional backward compatibility. Applications compiled decades ago can still run on modern Java runtimes, with no compromise in performance or security—thanks to Just-In-Time (JIT) compilation.

The reverse is also possible: Java code can run on older JDKs if compiled with the `--release` flag.

This flexibility allows Fedora to ship packages built with different Java versions, all working seamlessly together. There is no need for version-specific package duplication.

Additionally, users can run applications using a different Java runtime than the one used to build them. For example, Fedora-packaged applications can run with Adoptium JDKs from third-party repositories.


## Status in Fedora 41 and Earlier

- Multiple LTS versions of OpenJDK were packaged.
- One version was designated the “system Java.”
- Switching the system Java required a FESCo-approved change and coordinated porting by the Java SIG.


## Status in Fedora 42

- Only one LTS version (Java 21) is packaged.
- Java 21 is the de facto "system JDK" due to lack of alternatives.
- Some may argue there is no "system JDK" because there is no choice.


## Proposed Change for Fedora 43

- Introduce another LTS version—Java 25—alongside Java 21.
- Treat both LTS versions equally; neither is the default.
- Users and maintainers can choose which version to use.
- The most recent version will likely become the conventionally preferred one, but this is not enforced.


## What This Change Means for Fedora Users

- Greater flexibility in selecting the Java runtime.
- Freedom to mix and match Java versions for development and runtime.
- Ability to use Fedora Java packages with third-party JDKs.


## What This Means for Java Package Maintainers

- Packages built with Java 21 will remain on 21 until maintainers opt to migrate to Java 25.
- Migration can proceed gradually over multiple Fedora releases.
- Java SIG may assist in migrating packages when needed.
- Bugs will be filed for packages that are hard to migrate.
- Provenpackagers may fix some issues if maintainers are unresponsive.
- Unmaintained packages may be retired when their Java version is deprecated.

**Key Benefit:** Unported packages will no longer block the adoption of new Java versions. Updates will proceed more smoothly, and the Java SIG will no longer have to carry the burden of mass migration efforts alone.


## Runtime Strategies for Java Applications

Java applications in Fedora can follow one of these approaches:

1. **Generic Java dependency**  
   Use a generic “java” dependency – This allows compatibility with any implementation, including third-party JDKs. The actual runtime is chosen based on available repositories and system alternatives.
   _Examples: Tomcat, LibreOffice_

2. **Specific Java version dependency**  
   Tied to a single packaged Java runtime. Not affected by third-party JDKs or alternatives.  
   _Examples: JFlex, CUP_

3. **Version-specific subpackages**  
   Offers bindings for multiple Java versions; users choose the one they need.  
   _Examples: Maven, Ant_

Application maintainers know their software best and can choose the strategy that aligns with their users' needs.

**Note:** Java library packages are not affected by these runtime concerns, as they do not depend on any Java runtime.

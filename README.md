# code-quality
Common configurations files for code quality plugins: 

* PMD
* Checkstyle

## PMD

| Tool              | Version           | Link  |
| -------------     | -------------     | ----- |
| PMD               | 6.0.1 / 6.1.0     | https://pmd.github.io/ |
| Maven PMD Plugin  | 3.9               | https://maven.apache.org/components/plugins/maven-pmd-plugin/ |

### Usage

#### Gradle
```
apply plugin: 'pmd'

configurations {
    codeQualityConfig
}

dependencies {
    codeQualityConfig 'com.github.mstawowiak:code-quality:1.1.0'
}

pmd {
    ruleSets = []
    ruleSetConfig = resources.text.fromArchiveEntry(configurations.codeQualityConfig,
            "code-quality/static-code-analysis/pmd-ruleset.xml")
    consoleOutput = true
    sourceSets = [sourceSets.main]
    toolVersion = '6.1.0'
}
```

#### Maven
```
<properties>
    <java.target.version>1.8</java.target.version>
    <pmd.plugin.version>3.9</pmd.plugin.version>
    <code-quality.version>1.1.0</code-quality.version>
</properties>

<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-pmd-plugin</artifactId>
    <version>${pmd.plugin.version}</version>
    <dependencies>
        <dependency>
            <groupId>com.github.mstawowiak</groupId>
            <artifactId>code-quality</artifactId>
            <version>${code-quality.version}</version>
        </dependency>
    </dependencies>
    <configuration>
        <targetJdk>${java.target.version}</targetJdk>
        <includeTests>true</includeTests>
        <verbose>true</verbose>
        <linkXRef>false</linkXRef>
        <rulesets>
            <ruleset>code-quality/static-code-analysis/pmd-ruleset.xml</ruleset>
        </rulesets>
        <excludeRoots>
            <excludeRoot>${basedir}/target/generated-sources</excludeRoot>
        </excludeRoots>
    </configuration>
    <executions>
        <execution>
            <phase>validate</phase>
            <goals>
                <goal>check</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

## Checkstyle

| Tool                    | Version   | Link  |
| -------------           | --------- | ----- |
| Checkstyle              | 8.4       | http://checkstyle.sourceforge.net/ |
| Maven Checkstyle Plugin | 2.17      | https://maven.apache.org/plugins/maven-checkstyle-plugin/ |

### Usage

#### Gradle
```
apply plugin: 'checkstyle'

configurations {
    codeQualityConfig
}

dependencies {
    codeQualityConfig 'com.github.mstawowiak:code-quality:1.0.0'
}

checkstyle {
    config = resources.text.fromArchiveEntry(configurations.codeQualityConfig,
            "code-quality/static-code-analysis/checkstyle.xml")
    sourceSets = [sourceSets.main]
    toolVersion = '8.4'
}
```

#### Maven
```
<properties>
    <java.target.version>1.8</java.target.version>
    <checkstyle.plugin.version>2.17</checkstyle.plugin.version>
    <checkstyle.version>8.4</checkstyle.version>
    <code-quality.version>1.0.0</code-quality.version>
</properties>

<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-checkstyle-plugin</artifactId>
    <version>${checkstyle.plugin.version}</version>
    <dependencies>
        <dependency>
            <groupId>com.puppycrawl.tools</groupId>
            <artifactId>checkstyle</artifactId>
            <version>${checkstyle.version}</version>
        </dependency>
        <dependency>
            <groupId>com.github.mstawowiak</groupId>
            <artifactId>code-quality</artifactId>
            <version>${code-quality.version}</version>
        </dependency>
    </dependencies>
    <executions>
        <execution>
            <id>checkstyle-validate</id>
            <configuration>
                <excludes>**/target/**/*</excludes>
                <sourceDirectory>${project.basedir}</sourceDirectory>
                <configLocation>code-quality/static-code-analysis/checkstyle.xml</configLocation>
                <consoleOutput>true</consoleOutput>
                <failsOnError>true</failsOnError>
            </configuration>
            <phase>validate</phase>
            <goals>
                <goal>check</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```
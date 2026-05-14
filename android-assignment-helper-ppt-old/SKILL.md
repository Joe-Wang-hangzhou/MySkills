---
name: android-assignment-helper-ppt-old
description: Complete Android development course assignments from scratch. Use this skill whenever the user asks to create an Android Kotlin project based on a PowerPoint assignment description, build a university Android homework project, or implement a mobile app assignment for an Android development class. This skill reads PowerPoint files to extract assignment requirements, creates complete Android Kotlin projects using the "Empty Views Activity" template, embeds student initials "wch" in project and class names, and writes code in a simple, straightforward style typical of a university student without using advanced patterns or comments.
---

# Android Assignment Helper

This skill helps complete Android development course assignments. Given a PowerPoint file with assignment requirements, you will create a complete, working Android Kotlin project.

## Workflow

### Step 1: Extract Assignment Requirements from PowerPoint

Use the pptx skill to read the PowerPoint file and extract:
- Assignment title and description
- Required features and functionality
- Any screenshots or renderings showing expected UI
- Student initials requirement (look for "abc" or similar)

### Step 2: Create Android Project Structure

Create a new Android Kotlin project using the **"Empty Views Activity"** template in Android Studio. This template provides a basic Activity with a ConstraintLayout and is suitable for simple assignments.

Create the project in an appropriate directory with the following structure:

```
abc<AssignmentName>/
├── app/
│   ├── src/main/
│   │   ├── java/com/abc/<assignmentname>/
│   │   │   ├── MainActivity.kt
│   │   │   └── [other activities/classes]
│   │   ├── res/
│   │   │   ├── layout/
│   │   │   ├── values/
│   │   │   └── [other resources]
│   │   └── AndroidManifest.xml
│   └── build.gradle
├── build.gradle
└── settings.gradle
```

**Important**: The student initials "wch" must appear in:
- Project directory name: `wch<AssignmentName>`
- Package name: `com.wch.<assignmentname>`
- Class names: `Wch<Feature>Activity`, `Wch<Feature>Adapter`, etc.

### Step 3: Implement Features

Implement all required features from the assignment:
- Create necessary Activities, Fragments, Adapters
- Design layouts in XML
- Implement ViewModels and data handling
- Add any required resources (strings, colors, drawables)

### Step 4: Code Style Requirements

**CRITICAL - University Student Style**:
- Write simple, straightforward code
- Use basic Android patterns (Activity-only architecture is fine for simple assignments)
- NO advanced Kotlin features (no scope functions overuse, no DSL, no complex higher-order functions)
- Use explicit if/else statements instead of complex expressions
- Use simple variable names like `text1`, `btn`, `listView`
- Write code that looks like a typical undergraduate would write
- **DO NOT write any comments in the code**
- Avoid "cool" or "smart" one-liners
- Use verbose method names if unclear what to call something

### Step 5: Build and Verify (CRITICAL)

**You MUST run the build to verify before reporting completion.**

1. **Generate Gradle wrapper if missing**:
   ```bash
   cd <project_dir>
   gradle wrapper
   ```

2. **Run the build**:
   ```bash
   ./gradlew assembleDebug
   ```

3. **If build fails**, read the error carefully and fix the issue:
   - XML parsing error → Check for truncated/malformed XML files
   - Kotlin compilation error → Check syntax (missing braces, wrong package, etc.)
   - Resource error → Check resource file references
   - After fixing, run build again until it succeeds

4. **Only report success after `BUILD SUCCESSFUL`**

### Step 6: Additional Pre-Flight Checks

These checks run in parallel with the build process:

1. **Verify XML files are well-formed**:
   - Check ALL .xml files for proper opening and closing tags
   - Look for truncated files (common AI writing bug): if a file ends mid-tag like `</adaptive-icon` or `<adaptive-icon xmlns:android="http://schemas.android.com/ap`, the file is corrupted
   - If found, rewrite the complete, correct XML content

2. **Check for common issues**:
   - AndroidManifest.xml: intent-filters must have correct syntax
   - All `<receiver>` tags must be properly closed
   - All `<intent-filter>` tags inside receivers must have `android:priority` as an attribute, not a child element
   - Layout XML files must have properly closed root element

3. **Validate Kotlin syntax**:
   - Each .kt file must have matching braces `{}`
   - All `when` statements must have corresponding `}`
   - Package declaration must match directory structure

## Project Naming Convention

Always use "wch" as the student initials prefix:
- Directory: `wchCalculator`, `wchToDoList`, `wchWeatherApp`
- Package: `com.wch.calculator`, `com.wch.todolist`
- Classes: `WchMainActivity`, `WchCalculatorActivity`

## Code Examples

**GOOD (University Student Style)**:
```kotlin
class WchMainActivity : AppCompatActivity() {
    var count = 0
    val button: Button? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val plusBtn = findViewById<Button>(R.id.plusButton)
        val minusBtn = findViewById<Button>(R.id.minusButton)
        val textView = findViewById<TextView>(R.id.countText)

        plusBtn.setOnClickListener {
            count = count + 1
            textView.setText(count.toString())
        }

        minusBtn.setOnClickListener {
            count = count - 1
            textView.setText(count.toString())
        }
    }
}
```

**BAD (AI/Advanced Style - DO NOT USE)**:
```kotlin
class WchMainActivity : AppCompatActivity() {
    private val count = MutableStateFlow(0)
    private val plusBtn by lazy { findViewById<Button>(R.id.plusButton) }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        (plusBtn to findViewById<Button>(R.id.minusButton)).forEach { (btn, _) ->
            btn.setOnClickListener {
                count.update { if (btn.id == R.id.plusButton) it + 1 else it - 1 }
            }
        }
    }
}
```

## Gradle Configuration

Use standard, well-known Android Gradle configuration:
- `compileSdk = 34`
- `minSdk = 24`
- Standard dependencies (AndroidX, Material Components)
- No unusual or bleeding-edge library versions

## Output

Create a complete, working Android project that:
1. Opens in Android Studio
2. Builds successfully with `./gradlew assembleDebug`
3. Runs on an Android emulator showing the expected UI
4. Implements all features from the assignment

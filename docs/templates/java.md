# java templates

## junit 5 + hamcrest static imports

```java
package de.example;

import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.*;
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

class TestExample {

    ObjectUnderTest out;

    @BeforeEach
    public void setUp() {
        this.out = new ObjectUnderTest();
    }

    @Test
    void methodUnderTest_shouldReturnAllFields() {
        Integer count = out.methodUnderTest();
        assertNotNull(count);
        assertThat(count, is(greaterThan(300)));
    }
}

```

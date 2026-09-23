# Testing Requirements

## Minimum Test Coverage: 80%

Test Types (ALL required):
1. **Unit Tests** - Individual functions, utilities, components
2. **Integration Tests** - API endpoints, database operations
3. **E2E Tests** - Critical user flows (framework chosen per language)

## Test-Driven Development

MANDATORY workflow:
1. Write test first (RED)
2. Run test - it should FAIL
3. Write minimal implementation (GREEN)
4. Run test - it should PASS
5. Refactor (IMPROVE)
6. Verify coverage (80%+)

## Troubleshooting Test Failures

1. Use the **test-driven-development** skill (superpowers)
2. Check test isolation
3. Verify mocks are correct
4. Fix implementation, not tests (unless tests are wrong)

## Agent Support

- The **test-driven-development** skill (superpowers) - use PROACTIVELY for new features, enforces write-tests-first

## Test Structure (AAA Pattern)

Prefer Arrange-Act-Assert structure for tests (see [csharp/testing.md](../csharp/testing.md) for the xUnit/FluentAssertions framework choice):

```csharp
[Fact]
public void CalculatesSimilarity_ReturnsZero_ForOrthogonalVectors()
{
    // Arrange
    var vector1 = new[] { 1, 0, 0 };
    var vector2 = new[] { 0, 1, 0 };

    // Act
    var similarity = CosineSimilarity.Calculate(vector1, vector2);

    // Assert
    similarity.Should().Be(0);
}
```

### Test Naming

Use descriptive names that explain the behavior under test:

```csharp
[Fact]
public void FindCandidates_ReturnsEmptyList_WhenNoCardsMatchQuery() { }

[Fact]
public void Constructor_ThrowsArgumentException_WhenApiKeyIsMissing() { }

[Fact]
public void SearchByName_FallsBackToSubstringSearch_WhenIndexIsUnavailable() { }
```

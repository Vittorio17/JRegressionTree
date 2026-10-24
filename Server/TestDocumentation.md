# Test Documentation

## Unit Tests

Tests performed in isolation, without access to the file system, database, or network sockets.

### Package: `data`

#### Tested Class: `ContinuousAttribute`

**Test 1 (`testGetName`)**

**Purpose:** Verifies compliance with the principle of code reuse through inheritance: the continuous attribute correctly inherits the `getName()` behavior from the common superclass, returning its assigned symbolic name without duplicating logic.

| Field | Content |
|---|---|
| Input | `name = "chilometri"` passed to the constructor |
| Action | `ca.getName()` (inherited method) |
| Expected Result | Returns exactly `"chilometri"` |

**Test 2 (`testGetIndex`)**

**Purpose:** Verifies that encapsulation of the internal state correctly preserves the attribute's identifying index, which is stored and returned consistently with the value supplied during construction.

| Field | Content |
|---|---|
| Input | `index = 2` passed to the constructor |
| Action | `ca.getIndex()` |
| Expected Result | Returns `2` |

#### Tested Class: `DiscreteAttribute`

**Test 1 (`testNameAndIndex`)**

**Purpose:** Verifies that inheritance within the `Attribute` hierarchy ensures the correct initialization of both the name and index for the discrete subclass.

| Field | Content |
|---|---|
| Input | `name` and `index` passed to the constructor |
| Action | `da.getName()` and `da.getIndex()` |
| Expected Result | Both methods return the values passed to the constructor |

**Test 2 (`testNumberOfDistinctValues`)**

**Purpose:** Verifies that the number of distinct values managed by the discrete attribute accurately reflects the inserted elements, confirming the integrity of the underlying data structure.

| Field | Content |
|---|---|
| Input | A set of discrete values passed to the constructor |
| Action | `da.getNumberOfDistinctValue()` |
| Expected Result | Returns the exact number of inserted elements |

**Test 3 (`testEliminazioneValoriDuplicati`)**

**Purpose:** Verifies an architectural design choice: using `TreeSet` as the internal data structure guarantees unique values and automatically eliminates duplicates, ensuring that the reported cardinality includes only genuinely distinct elements.

| Field | Content |
|---|---|
| Input | An array containing repeated values, e.g. `["Fiat", "Fiat", "Ford"]` |
| Action | Construct the attribute using the provided array, then call `da.getNumberOfDistinctValue()` |
| Expected Result | Returns `2`; duplicate values are not counted more than once |

**Test 4 (`testIteratorReturnsValues`)**

**Purpose:** Verifies correct implementation of the `Iterable` interface contract, which allows all values stored in the attribute to be traversed uniformly and polymorphically.

| Field | Content |
|---|---|
| Input | A discrete attribute populated with a known set of values |
| Action | Execute a for-each loop over `da` |
| Expected Result | The iterator traverses all stored values without omissions |

#### Tested Class: `Attribute`

**Test 1 (`testGetName`)**

**Purpose:** Verifies the encapsulation principle underlying the class: the symbolic attribute name is correctly stored and returned.

| Field | Content |
|---|---|
| Input | `name = "temperatura"` passed to the constructor |
| Action | `a.getName()` |
| Expected Result | Returns exactly `"temperatura"` |

**Test 2 (`testGetIndex`)**

**Purpose:** Verifies encapsulation of the attribute's positional numeric identifier, which must be stored and returned correctly.

| Field | Content |
|---|---|
| Input | `index = 5` passed to the constructor |
| Action | `a.getIndex()` |
| Expected Result | Returns `5` |

#### Tested Class: `TrainingDataException`

**Test 1 (`testExceptionMessage`)**

**Purpose:** Verifies controlled error propagation: the diagnostic message is correctly passed through the exception hierarchy, ensuring that errors can be traced.

| Field | Content |
|---|---|
| Input | A test diagnostic string passed to the constructor |
| Action | Instantiate `new TrainingDataException(message)`, then call the inherited `getMessage()` method |
| Expected Result | `getMessage()` returns exactly the text supplied to the constructor |

### Package: `database`

#### Tested Class: `Column`

**Test 1 (`testGetColumnName`)**

**Purpose:** Verifies encapsulation: the column name is correctly stored and returned.

| Field | Content |
|---|---|
| Input | A column name passed to the constructor |
| Action | `col.getColumnName()` |
| Expected Result | Returns exactly the value assigned through the constructor |

**Test 2 (`testIsNumberTrue` and `testIsNumberFalse`)**

**Purpose:** Verifies correct data-type recognition, which is essential to the subsequent mapping of columns to discrete or continuous attributes.

| Field | Content |
|---|---|
| Input | A column with type `"number"` (true case) and a column with type `"string"` (false case) |
| Action | `col.isNumber()` |
| Expected Result | Returns `true` for type `"number"` and `false` for type `"string"` |

**Test 3 (`testUnknownTypeIsNotNumber`)**

**Purpose:** Verifies robustness against invalid input: an unrecognized column type must not be incorrectly classified as numeric.

| Field | Content |
|---|---|
| Input | A column with a non-standard type, e.g. `"date"` |
| Action | `col.isNumber()` |
| Expected Result | Returns `false` |

**Test 4 (`testToString`)**

**Purpose:** Verifies that the column's textual representation follows the expected standard format and complies with the project's representation contract.

| Field | Content |
|---|---|
| Input | A column with both its name and type set |
| Action | `col.toString()` |
| Expected Result | Returns a string formatted according to the `name:type` pattern |

#### Tested Class: `Example`

**Test 1 (`testAddAndGet`)**

**Purpose:** Verifies preservation of element order and type: the insertion order of tuple elements is maintained, and each element can be retrieved using its correct index while preserving its original type.

| Field | Content |
|---|---|
| Input | A string and a floating-point value inserted in sequence |
| Action | Call `ex.add(...)` for each element, then `ex.get(int)` for each index |
| Expected Result | `get(i)` returns the expected element at index `i`, preserving its original type |

**Test 2 (`testGetOutOfBounds`)**

**Purpose:** Verifies handling of invalid input: accessing an index that does not exist in the tuple is correctly detected, respecting the bounds of the internal list.

| Field | Content |
|---|---|
| Input | An invalid index outside the bounds of the internal list |
| Action | `ex.get(invalidIndex)` |
| Expected Result | An `IndexOutOfBoundsException` is thrown |

**Test 3 (`testToString`)**

**Purpose:** Verifies that the tuple's textual representation is readable and consistent with the domain objects' representation contract.

| Field | Content |
|---|---|
| Input | An `ex` tuple populated with multiple values |
| Action | `ex.toString()` |
| Expected Result | Returns a sequence of elements separated by spaces, containing all inserted values |

**Test 4 (`testCompareToEqual` and `testCompareToDifferent`)**

**Purpose:** Verifies correct implementation of the `Comparable` interface contract: identical tuples are considered equivalent, while tuples differing in at least one value are not.

| Field | Content |
|---|---|
| Input | Two identical tuples (equal case) and two tuples differing at one position (different case) |
| Action | `ex1.compareTo(ex2)` |
| Expected Result | Returns `0` for identical tuples and a non-zero value when a difference exists |

**Test 5 (`testIterator`)**

**Purpose:** Verifies compliance with the `Iterable` interface contract, allowing all elements stored in the tuple to be traversed uniformly.

| Field | Content |
|---|---|
| Input | An `ex` tuple populated with a known set of elements |
| Action | Execute a for-each loop over `ex` |
| Expected Result | All stored elements are processed correctly, without omissions |

#### Tested Class: `DatabaseConnectionException`

**Test 1 (`testMessage`)**

**Purpose:** Verifies that the diagnostic error message is correctly stored and returned.

| Field | Content |
|---|---|
| Input | A descriptive message passed to the constructor |
| Action | Instantiate `new DatabaseConnectionException(message)`, then call `getMessage()` |
| Expected Result | `getMessage()` returns exactly the supplied string |

**Test 2 (`testIsException`)**

**Purpose:** Verifies that the class is correctly positioned within Java's standard exception hierarchy and is therefore a checked exception.

| Field | Content |
|---|---|
| Input | An instance of `DatabaseConnectionException` |
| Action | Type assertion using `instanceof` |
| Expected Result | The instance is recognized as a subclass of `java.lang.Exception` |

#### Tested Class: `EmptySetException`

**Test 1 (`testMessage`)**

**Purpose:** Verifies that an exception instantiated without arguments exposes a standard, predefined diagnostic message, ensuring consistency in the error contract.

| Field | Content |
|---|---|
| Input | None; the no-argument constructor is used |
| Action | Instantiate `new EmptySetException()`, then call `getMessage()` |
| Expected Result | `getMessage()` returns exactly `"[!] The result set is empty"` |

**Test 2 (`testIsException`)**

**Purpose:** Verifies that the class correctly inherits from `java.lang.Exception`, ensuring its behavior as a checked exception.

| Field | Content |
|---|---|
| Input | An instance of `EmptySetException` |
| Action | Type assertion using `instanceof` |
| Expected Result | The instance is recognized as a subclass of `java.lang.Exception` |

#### Tested Class: `UnknownValueException`

**Test 1 (`testMessage`)**

**Purpose:** Verifies that the diagnostic error message is correctly initialized and preserved, consistently with the encapsulation principle used in the exception hierarchy.

| Field | Content |
|---|---|
| Input | A specific string passed to the constructor |
| Action | Instantiate `new UnknownValueException(message)`, then call `getMessage()` |
| Expected Result | `getMessage()` returns exactly the originally supplied text |

**Test 2 (`testIsException`)**

**Purpose:** Verifies that the class directly extends `java.lang.Exception`, making it a checked exception and requiring callers to handle it explicitly.

| Field | Content |
|---|---|
| Input | An instance of `UnknownValueException` |
| Action | Type assertion using `instanceof` |
| Expected Result | The instance is recognized as a direct subclass of `java.lang.Exception` |

---

## Integration Tests

Tests that verify interactions between internal modules or with external systems, such as a MySQL database.

### Package: `tree`

#### Tested Class: `DiscreteNode`

**Test 1 (`testGenerazioneRami`)**

**Purpose:** Verifies dynamic structure generation: the node automatically creates one branch for each distinct value assumed by the discrete attribute used for the split.

| Field | Content |
|---|---|
| Input | Data from discrete column `X`, containing distinct values `"A"` and `"B"` |
| Action | Construct a `DiscreteNode` using the supplied data |
| Expected Result | Exactly two distinct branches are generated, one for each value |

**Test 2 (`testCondizioneValoreEsistente`)**

**Purpose:** Verifies that, given a known input value, the node correctly identifies the corresponding split branch.

| Field | Content |
|---|---|
| Input | Existing value `"A"` |
| Action | `node.testCondition("A")` |
| Expected Result | Returns a valid index (`0` or `1`) corresponding to the branch generated for that value |

**Test 3 (`testCondizioneValoreInesistente`)**

**Purpose:** Verifies robust handling of invalid input: a value not present in the original training set must not produce an incorrect match.

| Field | Content |
|---|---|
| Input | An unmapped text value, e.g. `"Z"` |
| Action | `node.testCondition("Z")` |
| Expected Result | Returns `-1` |

#### Tested Class: `ContinuousNode`

**Test 1 (`testGenerazioneRami`)**

**Purpose:** Verifies that the variance-minimization algorithm identifies a valid threshold for a continuous attribute and generates the resulting child branches.

| Field | Content |
|---|---|
| Input | Numeric data from column `Y` |
| Action | Construct a `ContinuousNode` using the supplied data |
| Expected Result | A valid split threshold is identified, and at least one child branch is generated |

**Test 2 (`testValutazioneSoglia`)**

**Purpose:** Verifies that a continuous input value is routed correctly using the relational operators defined by the node's split threshold.

| Field | Content |
|---|---|
| Input | A numeric input value, e.g. `1.0` |
| Action | `node.testCondition(1.0)` |
| Expected Result | The returned index correctly identifies the corresponding partition branch, or returns `-1` if no branch matches |

#### Tested Class: `LeafNode`

**Test 1 (`testCalcoloValorePredetto`)**

**Purpose:** Verifies the arithmetic correctness of calculating the mean of the target values assigned to a leaf, a central component of the statistical aggregation used for prediction.

| Field | Content |
|---|---|
| Input | A set of `Example` instances assigned to the leaf, with numeric target values at indices `[beginExampleIndex, endExampleIndex]` |
| Action | Construct a `LeafNode` using the supplied examples, then call `leaf.getPredictedClassValue()` |
| Expected Result | The returned value is a valid floating-point number: `!Double.isNaN(value)` |

**Test 2 (`testNumeroRamiFigli`)**

**Purpose:** Verifies that a leaf is a terminal element in the node hierarchy and therefore cannot have child branches.

| Field | Content |
|---|---|
| Input | A `LeafNode` instance |
| Action | `leaf.getNumberOfChildren()` |
| Expected Result | Returns `0` |

**Test 3 (`testToString`)**

**Purpose:** Verifies that the leaf node's textual representation explicitly identifies it as a leaf, consistently with the representation contract shared by the `Node` hierarchy.

| Field | Content |
|---|---|
| Input | A `LeafNode` instance |
| Action | `leaf.toString()` |
| Expected Result | The generated string explicitly contains the keyword `"LEAF"` |

#### Tested Class: `Node` (tested through the concrete subclass `LeafNode`)

**Test 1 (`testInizializzazioneIndici`)**

**Purpose:** Verifies encapsulation of the hierarchy's shared state: the boundaries of the example interval covered by the node are correctly assigned and can be retrieved.

| Field | Content |
|---|---|
| Input | `trainingSet`, `beginExampleIndex`, and `endExampleIndex` passed to the `Node` constructor through `LeafNode` |
| Action | `node.getBeginExampleIndex()` and `node.getEndExampleIndex()` |
| Expected Result | Both methods return exactly the `beginExampleIndex` and `endExampleIndex` values supplied to the constructor |

**Test 2 (`testGenerazioneIdProgressivo`)**

**Purpose:** Verifies unique identification through a shared counter: node identifiers are generated progressively, ensuring increasing IDs for nodes created in sequence.

| Field | Content |
|---|---|
| Input | A first node created during setup, e.g. a `LeafNode`, followed by a second node |
| Action | Compare `secondNode.getIdNode()` with `firstNode.getIdNode()`; the `idNodeCount` counter is incremented at each construction |
| Expected Result | `secondNode.getIdNode() > firstNode.getIdNode()` |

**Test 3 (`testCalcoloVarianza`)**

**Purpose:** Verifies the mathematical consistency of the variance calculation performed in the node constructor, which underpins the regression tree's splitting strategy.

| Field | Content |
|---|---|
| Input | A `trainingSet` containing the examples covered by the node, within `[beginExampleIndex, endExampleIndex]` |
| Action | Construct the node; internally, the node calculates the SSE relative to the mean of `getClassValue()`. Then call `node.getVariance()` |
| Expected Result | `node.getVariance() >= 0.0` |

**Test 4 (`testToString`)**

**Purpose:** Verifies that the node's descriptive string includes the key information required for diagnostics, consistently with the representation contract shared by the hierarchy.

| Field | Content |
|---|---|
| Input | A node whose `beginExampleIndex`, `endExampleIndex`, and variance have already been calculated |
| Action | `node.toString()` |
| Expected Result | Returns a string in the exact format `"Nodo: [Examples:<begin>-<end>] variance:<variance>"` |

#### Tested Class: `SplitNode` (tested through the concrete implementation `DiscreteNode`)

**Test 1 (`testGetAttribute`)**

**Purpose:** Verifies encapsulation of the attribute on which the split is based, ensuring that the original attribute instance can be retrieved faithfully.

| Field | Content |
|---|---|
| Input | An `Attribute` instance, e.g. a `DiscreteAttribute`, passed to the `DiscreteNode` or `ContinuousNode` constructor |
| Action | `splitNode.getAttribute()` |
| Expected Result | Returns exactly the `Attribute` instance passed during construction, stored in the private `attribute` field |

**Test 2 (`testCalcoloSplitVariance`)**

**Purpose:** Verifies the mathematical validity of the overall split variance, calculated as the sum of the variances of the child branches, which underpins the tree's recursive construction strategy.

| Field | Content |
|---|---|
| Input | A training data subset covering `[beginExampleIndex, endExampleIndex]`, used to generate child branches |
| Action | Construct the node. The `SplitNode` constructor sorts the training set, calls `setSplitInfo()`, and sums the variance of a temporary `LeafNode` created for each branch in `mapSplit`. Then call `splitNode.getVariance()` |
| Expected Result | `splitNode.getVariance()` (`splitVariance`) returns a real number greater than or equal to `0.0` |

**Test 3 (`testGestioneFigliESplitInfo`)**

**Purpose:** Verifies that the child-branch data structure (`mapSplit`) is populated correctly and that the associated `SplitInfo` objects are accessible.

| Field | Content |
|---|---|
| Input | A split node with child branches already generated and stored in the protected `mapSplit` list |
| Action | Call `splitNode.getNumberOfChildren()` (returns `mapSplit.size()`) and `splitNode.getSplitInfo(0)` (returns `mapSplit.get(0)`) |
| Expected Result | `getNumberOfChildren()` is greater than `0`, confirming the presence of branches; `getSplitInfo(0)` returns a non-null `SplitNode.SplitInfo` instance |

**Test 4 (`testFormulateQuery`)**

**Purpose:** Verifies that the node can export the test conditions associated with its branches as text, enabling split rules to be communicated to the server through a shared textual format.

| Field | Content |
|---|---|
| Input | A split node with child branches configured in `mapSplit` |
| Action | `splitNode.formulateQuery()` |
| Expected Result | The resulting string concatenates one line for each branch `i`, in the format `"<i>: <attributeName><comparator><splitValue>\n"`, including the split attribute's name, comparator, and value for each branch |

**Test 5 (`testCompareTo`)**

**Purpose:** Verifies correct implementation of the `Comparable` interface contract, ensuring that comparisons between nodes with equivalent variances are consistent.

| Field | Content |
|---|---|
| Input | A second node with characteristics and data similar to the first, producing an equivalent variance |
| Action | `node1.compareTo(node2)` |
| Expected Result | Returns `0` when `node1.getVariance() == node2.getVariance()`, `-1` when `node1.getVariance() < node2.getVariance()`, and `1` when `node1.getVariance() > node2.getVariance()` |

#### Tested Class: `RegressionTree`

**Test 1 (`testInduzioneAlbero`)**

**Purpose:** Verifies recursive composition: the regression tree is correctly constructed from the training dataset.

| Field | Content |
|---|---|
| Input | A training dataset loaded from the `provaC` table |
| Action | `new RegressionTree(trainingSet)`, which internally invokes `learnTree()` and recursively creates `LeafNode` or `SplitNode` instances based on the `numberOfExamplesPerLeaf` threshold, set to 10% of the examples |
| Expected Result | The constructor correctly instantiates the node hierarchy (`root` and, if the root is not a leaf, `childTree[]`). `tree.toString()` is not `null` and contains `"SPLIT"` (from `SplitNode.toString()`) and/or `"LEAF"` (from `LeafNode.toString()`) |

**Test 2 (`testComportamentoNodoRadice`)**

**Purpose:** Verifies the polymorphic behavior of data-retrieval methods when called on the root node, correctly distinguishing between a decision node and a leaf node.

| Field | Content |
|---|---|
| Input | A tree generated from `provaC`, whose root is a non-terminal decision node |
| Action | `tree.getCurrentNodeQuery()` and `tree.getPredictedValue()` |
| Expected Result | `getCurrentNodeQuery()` returns a valid string containing the split condition; `getPredictedValue()` returns `null`, as expected for a non-leaf node |

**Test 3 (`testNavigazioneEccezione`)**

**Purpose:** Verifies error handling when navigation attempts to access a nonexistent child branch: the custom `UnknownValueException` must be thrown.

| Field | Content |
|---|---|
| Input | An invalid index, e.g. `-1` |
| Action | `tree.getChild(-1)` |
| Expected Result | The custom `UnknownValueException` is thrown |

**Test 4 (`testSerializzazione`)**

**Purpose:** Verifies compliance with the `Serializable` interface contract, ensuring that the tree can be persisted to a file and restored with its original structure intact.

| Field | Content |
|---|---|
| Input | A constructed `RegressionTree` instance and a temporary local file path, `"test_tree.ser"` |
| Action | Call `tree.salva("test_tree.ser")` to serialize the object using `ObjectOutputStream`, followed by `RegressionTree.carica("test_tree.ser")`, which deserializes it using `ObjectInputStream` |
| Expected Result | The file `"test_tree.ser"` is created, and the tree returned by `carica()` produces the same `toString()` output as the original tree |

### Package: `data`

#### Tested Class: `Data`

**Test 1 (`tabellaInesistente_throwsTrainingDataException`)**

**Purpose:** Verifies handling of invalid input: attempting to access a nonexistent table is handled in a controlled manner by throwing an application-specific exception.

| Field | Content |
|---|---|
| Input | The name of a table that does not exist in the database |
| Action | `new Data(nonexistentTableName)` |
| Expected Result | A `TrainingDataException` is thrown |

**Test 2 (`tabellaConSingolaColonna_throwsTrainingDataException`)**

**Purpose:** Verifies structural schema validation, ensuring that a table contains at least one explanatory attribute and one target attribute.

| Field | Content |
|---|---|
| Input | A table containing fewer than two columns |
| Action | `new Data(tableName)` |
| Expected Result | A `TrainingDataException` is thrown |

**Test 3 (`targetNonNumerico_throwsTrainingDataException`)**

**Purpose:** Verifies that the domain constraint requiring a numeric continuous class attribute, necessary for regression trees, is enforced during data loading.

| Field | Content |
|---|---|
| Input | A table whose last column (the target) is not a numeric continuous type |
| Action | `new Data(tableName)` |
| Expected Result | A `TrainingDataException` is thrown |

**Test 4 (`identificaTipoColonnaCorrettamente`)**

**Purpose:** Verifies dynamic mapping of database column types to the corresponding attribute types, distinguishing discrete from continuous explanatory attributes.

| Field | Content |
|---|---|
| Input | A dataset loaded from `provaC`, where column `X` is textual at index `0` and column `Y` is numeric at index `1` |
| Action | Call `data.getExplanatoryAttribute(0)` and `data.getExplanatoryAttribute(1)`. The constructor populates the explanatory attributes according to `Column.isNumber()` |
| Expected Result | `getExplanatoryAttribute(0)` returns a `DiscreteAttribute` for column `X`; `getExplanatoryAttribute(1)` returns a `ContinuousAttribute` for column `Y` |

**Test 5 (`getExplanatoryValue_ritornaValoreNonNullo` and `_throwsIndexOutOfBoundsException`)**

**Purpose:** Verifies the reliability of the loaded example matrix and, for invalid input, compliance with index bounds.

| Field | Content |
|---|---|
| Input | Valid row and column indices (non-null case) and out-of-range indices (exception case) |
| Action | `data.getExplanatoryValue(row, column)` |
| Expected Result | Valid indices return a non-null object; out-of-range indices cause an `IndexOutOfBoundsException` to be thrown |

**Test 6 (`getClassAttribute_attributoTargetCorretto` and `getClassValue_ritornaIndiceValido`)**

**Purpose:** Verifies the correct binding between the dataset's target variable and its internal representation, maintaining the separation between explanatory attributes and the class attribute.

| Field | Content |
|---|---|
| Input | A dataset whose last column is target column `C` |
| Action | Call `data.getClassAttribute()` (package-private, so the test must be in package `data`) and `data.getClassValue(row)` |
| Expected Result | `getClassAttribute()` returns the `ContinuousAttribute` corresponding to the last column, with name `"C"` and index `numAttributes - 1`; `getClassValue(row)` returns the target value for that row as a primitive `double` without throwing errors |

**Test 7 (`ordinamentoCorretto_discreteAttribute`)**

**Purpose:** Verifies the internal sorting algorithm (Quicksort) when applied to the textual values of a discrete attribute.

| Field | Content |
|---|---|
| Input | A dataset containing discrete column `X`, using the full interval `[0, data.getNumberOfExamples() - 1]` |
| Action | `data.sort(attributeX, 0, data.getNumberOfExamples() - 1)`. Internally, this invokes `quicksort()`, which uses `String.compareTo()` as the partitioning criterion for discrete attributes |
| Expected Result | After sorting, iterating through the rows with `getExplanatoryValue(i, indexX)` yields the values of column `X` in ascending alphabetical order |

**Test 8 (`ordinamentoCorretto_continuousAttribute`)**

**Purpose:** Verifies the internal sorting algorithm (Quicksort) when applied to the real values of a continuous attribute.

| Field | Content |
|---|---|
| Input | A dataset containing continuous column `Y`, using the full interval `[0, data.getNumberOfExamples() - 1]` |
| Action | `data.sort(attributeY, 0, data.getNumberOfExamples() - 1)`. Internally, this invokes `quicksort()`, which uses `Double.compareTo()` as the partitioning criterion for continuous attributes |
| Expected Result | After sorting, iterating through `getExplanatoryValue(i, indexY)` yields all values of column `Y` in ascending numeric order |

### Package: `database`

#### Tested Class: `TableData`

**Test 1 (`testGetTransazioniValide`)**

**Purpose:** Verifies that executing a `SELECT` query correctly produces an ordered list of `Example` tuples, consistently mapping database results to domain objects.

| Field | Content |
|---|---|
| Input | Table `"provaC"` |
| Action | `tableData.getTransazioni("provaC")` |
| Expected Result | The returned `List<Example>` is neither `null` nor empty |

**Test 2 (`testGetTransazioniTabellaInesistente`)**

**Purpose:** Verifies that retrieving data from a nonexistent table explicitly throws an `SQLException`.

| Field | Content |
|---|---|
| Input | The name of a nonexistent table |
| Action | `tableData.getTransazioni(nonexistentTableName)` |
| Expected Result | An `SQLException` is thrown. In the implementation described, `TableSchema` returns `getNumberOfAttributes() == 0` for a nonexistent table, causing `getTransazioni()` to explicitly throw `new SQLException()` before executing the query |

**Test 3 (`testGetDistinctColumnValuesString`)**

**Purpose:** Verifies the use of `TreeSet` to retrieve and sort unique string values from a database column.

| Field | Content |
|---|---|
| Input | Table `"provaC"` and a `Column` object representing textual column `X` |
| Action | `tableData.getDistinctColumnValues("provaC", columnX)`, which executes `SELECT DISTINCT X FROM provaC ORDER BY X ASC` |
| Expected Result | The returned `Set<Object>`, backed by a `TreeSet`, is not empty and contains values of type `String` |

**Test 4 (`testGetDistinctColumnValuesNumber`)**

**Purpose:** Verifies the use of `TreeSet` to retrieve and sort unique numeric (`Double`) values from a database column.

| Field | Content |
|---|---|
| Input | Table `"provaC"` and a `Column` object representing numeric column `Y` |
| Action | `tableData.getDistinctColumnValues("provaC", columnY)`. Since `columnY.isNumber() == true`, values are read using `rs.getDouble(1)` |
| Expected Result | The returned `Set<Object>` is not empty and contains values of type `Double` |

#### Tested Class: `TableSchema`

**Test 1 (`testGetNumberOfAttributes`)**

**Purpose:** Verifies the correct extraction and counting of columns from the database data dictionary, which underpins schema construction.

| Field | Content |
|---|---|
| Input | A connected database and an existing table, e.g. `"provaC"` |
| Action | Instantiate `new TableSchema(db, "provaC")`, then call `schema.getNumberOfAttributes()` |
| Expected Result | Returns a value strictly greater than zero |

**Test 2 (`testGetColumn` and `testGetColumnOutOfBounds`)**

**Purpose:** Verifies positional access to a column's type definition and correct handling of requests using indices that do not exist in the schema.

| Field | Content |
|---|---|
| Input | A valid index (for `getColumn`) and an index outside the schema bounds (out-of-bounds case) |
| Action | `schema.getColumn(index)` |
| Expected Result | A valid index returns the corresponding `Column` object; an invalid index causes an `IndexOutOfBoundsException` to be thrown |

**Test 3 (`testIterator`)**

**Purpose:** Verifies compliance with the `Iterable` interface contract, allowing all columns in the schema to be traversed sequentially.

| Field | Content |
|---|---|
| Input | A schema populated with a known set of columns |
| Action | Execute a for-each loop over `schema` |
| Expected Result | The number of loop iterations exactly matches the number of attributes |

**Test 4 (`testTabellaInesistente`)**

**Purpose:** Verifies robust handling of metadata retrieval for a table that is not defined in the database, tolerating invalid external data without propagating uncontrolled errors.

| Field | Content |
|---|---|
| Input | A fictitious table name that does not exist |
| Action | `new TableSchema(nonexistentTableName)` |
| Expected Result | The invalid input is handled without propagating uncontrolled errors: the `ResultSet` is simply exhausted, and the internal column list remains empty, with cardinality `0` |

#### Tested Class: `DbAccess`

**Test 1 (`testInitAndGetConnection`)**

**Purpose:** Verifies initialization of the MySQL database connection and retrieval of the active connection, centralizing session management.

| Field | Content |
|---|---|
| Input | Network parameters hard-coded in the class: `SERVER = "localhost"`, `PORT = 3306`, `DATABASE = "MapDB"`, `USER_ID = "MapUser"` |
| Action | `dbAccess.initConnection()`, which loads the `com.mysql.cj.jdbc.Driver` driver and calls `DriverManager.getConnection()`, followed by `dbAccess.getConnection()` (package-private, so the test must be in package `database`) |
| Expected Result | The returned `Connection` object is not `null`, `connection.isClosed() == false`, and no `DatabaseConnectionException` is thrown |

**Test 2 (`testCloseConnection`)**

**Purpose:** Verifies that the database session is closed correctly and its allocated network resources are released, preventing resource leaks and dangling connections.

| Field | Content |
|---|---|
| Input | An already established database connection |
| Action | `dbAccess.closeConnection()` |
| Expected Result | The underlying `Connection` is closed: `connection.isClosed() == true` |

### Package: `server`

#### Tested Class: `MultiServer`

**Test 1 (`testServerAvvioEConnessione`)**

**Purpose:** Verifies concurrent execution: the server starts correctly on a dedicated thread and can accept incoming connection requests.

| Field | Content |
|---|---|
| Input | A server started on a test port in a secondary thread |
| Action | Create a client `Socket` and attempt to connect to the server |
| Expected Result | `assertTrue` confirms that the connection is successfully established and that the server is listening for incoming connections |

#### Tested Class: `ServerOneClient`

**Test 1 (`testAcquisizioneDatiOK`)**

**Purpose:** Verifies processing of command `0` (data acquisition) and correct instantiation of `ServerOneClient` as a dedicated thread for the connected client, following the one-thread-per-client architectural model.

| Field | Content |
|---|---|
| Input | A temporary `ServerSocket`; the client sends the integer `0` and the string `"provaC"` through an `ObjectOutputStream` |
| Action | Instantiate and start `new ServerOneClient(connectedSocket)`, then send the input data described above |
| Expected Result | The response received from the server is exactly `"OK"` |

**Test 2 (`testCaricamentoAlberoFallito`)**

**Purpose:** Verifies handling of invalid input: a request to load a nonexistent `.dmp` archive using command `2` is handled correctly, and the error is communicated to the client through explicit exception handling.

| Field | Content |
|---|---|
| Input | Integer command `2`, followed by the name of a table that has not previously been serialized |
| Action | Send the command and table name to `ServerOneClient` |
| Expected Result | The internal `catch` block is triggered, and the hard-coded error string `"Errore durante il caricamento da archivio."` is returned |

## Diagonal Matrix

```cpp
class DiagonalMatrix {  
    int rowColSize;  
    int* diagonal;  
public:  
    DiagonalMatrix(const int& nBynSize) :  
        rowColSize(nBynSize), diagonal(new int[nBynSize]{}) {}  
  
    void setElement(const int& row, const int& column, const int& element) {  
        if (row == column && row < this->rowColSize)  
            *(this->diagonal + row) = element;  
    }  
    
    int getElement(const int& row, const int& column) {  
        if (row == column && row < this->rowColSize)  
            return *(this->diagonal + row);  
        return 0;  
    }  
    
    ~DiagonalMatrix() {  
        delete[] this->diagonal;  
    }  
    
    friend std::ostream& operator<<(std::ostream& os, const DiagonalMatrix& diagonalMatrix);  
};  
  
std::ostream& operator<<(std::ostream& os, const DiagonalMatrix& diagonalMatrix) {  
    for (int rowIndex = 0; rowIndex < diagonalMatrix.rowColSize; ++rowIndex) {  
        for (int colIndex = 0; colIndex < diagonalMatrix.rowColSize; ++colIndex)  
            os << (rowIndex == colIndex ? *(diagonalMatrix.diagonal + rowIndex) : 0) << ' ';  
        os << '\n';  
    }  
    return os;  
}  
  
int main() {  
    DiagonalMatrix diagonalMatrix{5};  
  
    diagonalMatrix.setElement(0, 0, 10);  
    diagonalMatrix.setElement(1, 1, 20);  
    diagonalMatrix.setElement(2, 2, 30);  
    diagonalMatrix.setElement(3, 3, 40);  
    diagonalMatrix.setElement(4, 4, 50);  
  
    std::cout << diagonalMatrix;  
  
    return 0;  
}

// [1, 0, 0]
// [0, 2, 0]
// [0, 0, 3]
```

## Lower Triangular Matrix Row-major

```cpp
class LowerTriangularMatrix {  
    int rowColSize;  
    int* elements;  
public:  
    LowerTriangularMatrix(const int& nBynSize) :  
        rowColSize(nBynSize), elements(new int[(nBynSize * (nBynSize + 1)) / 2]{}) {}  
  
    void setElement(const int& row, const int& column, const int& element) {  
        if (column <= row && row < this->rowColSize)  
            *(this->elements + ((row + 1) * row / 2) + column) = element;  
    }  
    
    int getElement(const int& row, const int& column) {  
        if (column <= row && row < this->rowColSize)  
            return *(this->elements + ((row + 1) * row / 2) + column);  
  
        return 0;  
    }  
    
    ~LowerTriangularMatrix() {  
        delete[] this->elements;  
    }  
    
    friend std::ostream& operator<<(std::ostream& os, const LowerTriangularMatrix& lowerTriangularMatrix);  
};  
  
std::ostream& operator<<(std::ostream& os, const LowerTriangularMatrix& lowerTriangularMatrix) {  
    for (int rowIndex = 0; rowIndex < lowerTriangularMatrix.rowColSize; ++rowIndex) {  
        for (int colIndex = 0; colIndex < lowerTriangularMatrix.rowColSize; ++colIndex)  
            os << (colIndex <= rowIndex ? *(lowerTriangularMatrix.elements + ((rowIndex + 1) * rowIndex / 2) + colIndex) : 0) << ' ';  
        os << '\n';  
    }  
    return os;  
}  
  
int main() {  
    LowerTriangularMatrix lowerTriangularMatrix{5};  
  
    lowerTriangularMatrix.setElement(0, 0, 10);  
    lowerTriangularMatrix.setElement(1, 0, 20);  
    lowerTriangularMatrix.setElement(1, 1, 30);  
    lowerTriangularMatrix.setElement(2, 0, 40);  
    lowerTriangularMatrix.setElement(2, 1, 50);  
    lowerTriangularMatrix.setElement(2, 2, 60);  
    lowerTriangularMatrix.setElement(3, 0, 70);  
    lowerTriangularMatrix.setElement(3, 1, 80);  
    lowerTriangularMatrix.setElement(3, 2, 90);  
    lowerTriangularMatrix.setElement(3, 3, 100);  
    lowerTriangularMatrix.setElement(4, 0, 110);  
    lowerTriangularMatrix.setElement(4, 1, 120);  
    lowerTriangularMatrix.setElement(4, 2, 130);  
    lowerTriangularMatrix.setElement(4, 3, 140);  
    lowerTriangularMatrix.setElement(4, 4, 150);  
  
    std::cout << lowerTriangularMatrix;  
  
    return 0;  
}

// [1, 0, 0]
// [2, 3, 0]
// [4, 5, 6]
```

## Lower Triangular Matrix Column-major

```cpp
class LowerTriangularMatrix {  
    int rowColSize;  
    int* elements;  
public:  
    LowerTriangularMatrix(const int& nBynSize) :  
        rowColSize(nBynSize), elements(new int[(nBynSize * (nBynSize + 1)) / 2]{}) {}  
  
    void setElement(const int& row, const int& column, const int& element) {
        //index = (row * (row + 1)) / 2 + column;
        int columnIterations = column;  
        int movesFromStart = 0;  
        int columnDecrement = 0;  
  
        while (columnIterations > 0) {  
            movesFromStart += (this->rowColSize - columnDecrement);  
            --columnIterations;  
            ++columnDecrement;  
        }  
        if (column <= row && row < this->rowColSize)  
            *(this->elements + movesFromStart + row - column) = element;  
    }  
    
    int getElement(const int& row, const int& column) {  
        int columnIterations = column;  
        int movesFromStart = 0;  
        int columnDecrement = 0;  
  
        while (columnIterations > 0) {  
            movesFromStart += (this->rowColSize - columnDecrement);  
            --columnIterations;  
            ++columnDecrement;  
        }  
        
        if (column <= row && row < this->rowColSize)  
            return *(this->elements + movesFromStart + row - column);  
  
        return 0;  
    }  
    
    ~LowerTriangularMatrix() {  
        delete[] this->elements;  
    }  
    
    friend std::ostream& operator<<(std::ostream& os, const LowerTriangularMatrix& lowerTriangularMatrix);  
};  
  
std::ostream& operator<<(std::ostream& os, const LowerTriangularMatrix& lowerTriangularMatrix) {  
    for (int rowIndex = 0; rowIndex < lowerTriangularMatrix.rowColSize; ++rowIndex) {  
        for (int colIndex = 0; colIndex < lowerTriangularMatrix.rowColSize; ++colIndex) {  
            if (colIndex <= rowIndex) {  
                int columnIterations = colIndex;  
                int movesFromStart = 0;  
                int columnDecrement = 0;  
  
                while (columnIterations > 0) {  
                    movesFromStart += (lowerTriangularMatrix.rowColSize - columnDecrement);  
                    --columnIterations;  
                    ++columnDecrement;  
                }                os << *(lowerTriangularMatrix.elements + movesFromStart + rowIndex - colIndex) << ' ';  
  
            } else {  
                os << 0 << ' ';  
            }        
		}        
		
		os << '\n';  
    }  
    return os;  
}  
  
int main() {  
    LowerTriangularMatrix lowerTriangularMatrix{5};  
  
    lowerTriangularMatrix.setElement(0, 0, 10);  
    lowerTriangularMatrix.setElement(1, 0, 20);  
    lowerTriangularMatrix.setElement(1, 1, 30);  
    lowerTriangularMatrix.setElement(2, 0, 40);  
    lowerTriangularMatrix.setElement(2, 1, 50);  
    lowerTriangularMatrix.setElement(2, 2, 60);  
    lowerTriangularMatrix.setElement(3, 0, 70);  
    lowerTriangularMatrix.setElement(3, 1, 80);  
    lowerTriangularMatrix.setElement(3, 2, 90);  
    lowerTriangularMatrix.setElement(3, 3, 100);  
    lowerTriangularMatrix.setElement(4, 0, 110);  
    lowerTriangularMatrix.setElement(4, 1, 120);  
    lowerTriangularMatrix.setElement(4, 2, 130);  
    lowerTriangularMatrix.setElement(4, 3, 140);  
    lowerTriangularMatrix.setElement(4, 4, 150);  
  
    std::cout << lowerTriangularMatrix;  
  
    return 0;  
}

// [1, 0, 0]
// [2, 4, 0]
// [3, 5, 6]
```

### Upper Triangular Matrix Row-Major & Column-Major

```cpp
class UpperTriangularMatrix {
    int rowColSize;  
    int* elements;  
public:  
    UpperTriangularMatrix(const int& nBynSize) :
        rowColSize(nBynSize), elements(new int[(nBynSize * (nBynSize + 1)) / 2]{}) {}  
  
    void setElement(const int& row, const int& column, const int& element) {
        int spacing = -1, rowCounter = row, spacingSum{};

        while (rowCounter > -1) {
            ++spacing;
            spacingSum += spacing;
            --rowCounter;
        }

        if (row <= column && column < this->rowColSize)
            *(elements + this->rowColSize * row + column - spacingSum) = element;
    }  
    
    int getElement(const int& row, const int& column) const {
        int spacing = -1, rowCounter = row, spacingSum{};

        while (rowCounter > -1) {
            ++spacing;
            spacingSum += spacing;
            --rowCounter;
        }

        if (row <= column && column < this->rowColSize)
            return *(elements + this->rowColSize * row + column - spacingSum);

        return 0;
    }
    
    ~UpperTriangularMatrix() {
        delete[] this->elements;  
    }  
    
    friend std::ostream& operator<<(std::ostream& os, const UpperTriangularMatrix& upperTriangularMatrix);
};  
  
std::ostream& operator<<(std::ostream& os, const UpperTriangularMatrix& upperTriangularMatrix) {
    for (int rowIndex = 0; rowIndex < upperTriangularMatrix.rowColSize; ++rowIndex) {
        for (int colIndex = 0; colIndex < upperTriangularMatrix.rowColSize; ++colIndex) {
            if (rowIndex <= colIndex) {
                int spacing = -1, rowCounter = rowIndex, spacingSum{};

                while (rowCounter > -1) {
                    ++spacing;
                    spacingSum += spacing;
                    --rowCounter;
                }

                os << (*(upperTriangularMatrix.elements + upperTriangularMatrix.rowColSize * rowIndex + colIndex - spacingSum)) << ' ';

            } else {
                os << 0 << ' ';
            }
        }
        os << '\n';
    }

    return os;  
}  
  
int main() {  
    UpperTriangularMatrix upperTriangularMatrix{5};

    upperTriangularMatrix.setElement(0, 4, 50);
    upperTriangularMatrix.setElement(3, 3, 10);
    upperTriangularMatrix.setElement(4, 4, 100);

    std::cout << upperTriangularMatrix;
  
    return 0;  
}
// Row Major
// [1, 2, 3]
// [0, 4, 5]
// [0, 0, 6]

// Column Major
// [1, 2, 4]
// [0, 3, 5]
// [0, 0, 6]
```

## Symmetrical Matrix

```cpp
// Create a Matrix will elements and display it
int main() {
    constexpr int matrixSize = 5;
    int** matrix = new int*[matrixSize];
    
    for (int i = 0; i < matrixSize; ++i)
        *(matrix + i) = new int[matrixSize];

    for (int i = 0; i < matrixSize; ++i)
        for (int j = 0; j < matrixSize; ++j)
            *(*(matrix + i) + j) = std::min(i, j) + 1;


    for (int row = 0; row < matrixSize; ++row) {
        for (int col = 0; col < matrixSize; ++col) {
            std::cout << *(*(matrix + row) + col) << ' ';
        }
        
        std::cout << '\n';
        delete matrix[row];
    }
    delete[] matrix;

    return 0;
}

void generateSymmetricalMatrixFromDiagonal(
	const int* diagonalArray, const int& diagonalLength
) {
    for (int row = 0; row < diagonalLength; ++row) {
        for (int col = 0; col < diagonalLength; ++col)
            std::cout << *(diagonalArray + std::min(row, col)) << ' ';
        std::cout << '\n';
    }
}

int main() {
    int diagonalSize;
    std::cout << "Enter diagonal of a symmetric matrix: ";
    std::cin >> diagonalSize;

    int* symmetricalMatrixDiagonal = new int[diagonalSize];

    for (int i = 0; i < diagonalSize; ++i) {
        std::cout << "Enter diagonal element " << i + 1 << ": ";
        std::cin >> *(symmetricalMatrixDiagonal + i);
    }

    generateSymmetricalMatrixFromDiagonal(symmetricalMatrixDiagonal, diagonalSize);

    delete[] symmetricalMatrixDiagonal;
}

class SymmetricMatrix {
    int rowColSize;
    int* uniqueElements;
public:
    SymmetricMatrix(const int& nBynSize) :
        rowColSize(nBynSize), uniqueElements(new int[nBynSize]{}) {}

    void setElement(const int& row, const int& column, const int& element) {
       if (row >= this->rowColSize || column >= this->rowColSize)
            return;

       *(this->uniqueElements + std::min(row, column)) = element;
    }

    int getElement(const int& row, const int& column) const {
       if (row >= this->rowColSize || column >= this->rowColSize)
           return -1;

        return *(this->uniqueElements + std::min(row, column));
    }

    ~SymmetricMatrix() {
        delete[] this->uniqueElements;
    }

    friend std::ostream& operator<<(std::ostream& os, const SymmetricMatrix& symmetricMatrix);
};

std::ostream& operator<<(std::ostream& os, const SymmetricMatrix& symmetricMatrix) {
    for (int rowIndex = 0; rowIndex < symmetricMatrix.rowColSize; ++rowIndex) {
        for (int colIndex = 0; colIndex < symmetricMatrix.rowColSize; ++colIndex)
            os << symmetricMatrix.getElement(rowIndex, colIndex) << ' ';
        os << '\n';
    }

    return os;
}

int main() {
    SymmetricMatrix symmetricMatrix{5};

    symmetricMatrix.setElement(0,0, 1);
    symmetricMatrix.setElement(1,1, 2);
    symmetricMatrix.setElement(2,2, 3);
    symmetricMatrix.setElement(3,3, 4);
    symmetricMatrix.setElement(4,4, 5);

    std::cout << symmetricMatrix;

    return 0;
}

// 1 1 1 1 1
// 1 2 2 2 2
// 1 2 3 3 3
// 1 2 3 4 4
// 1 2 3 4 5
```

## Non-Identical Symmetric Matrix

```cpp
class SymmetricalMatrix {
    int rowColSize;  
    int* elements;  
public:  
    SymmetricalMatrix(const int& nBynSize) :
        rowColSize(nBynSize), elements(new int[(nBynSize * (nBynSize + 1)) / 2]{}) {}  
  
    void setElement(const int& row, const int& column, const int& element) {  
        if (column <= row && row < this->rowColSize)  
            *(this->elements + ((row + 1) * row / 2) + column) = element;
        else
            *(this->elements + ((column + 1) * column / 2) + row) = element;
    }  
    
    int getElement(const int& row, const int& column) const {
        if (column <= row && row < this->rowColSize)  
            return *(this->elements + ((row + 1) * row / 2) + column);  
        else
            return *(this->elements + ((column + 1) * column / 2) + row);
    }
    
    ~SymmetricalMatrix() {
        delete[] this->elements;  
    }  
    
    friend std::ostream& operator<<(std::ostream& os, const SymmetricalMatrix& lowerTriangularMatrix);
};  
  
std::ostream& operator<<(std::ostream& os, const SymmetricalMatrix& lowerTriangularMatrix) {
    for (int rowIndex = 0; rowIndex < lowerTriangularMatrix.rowColSize; ++rowIndex) {  
        for (int colIndex = 0; colIndex < lowerTriangularMatrix.rowColSize; ++colIndex)  
            os << lowerTriangularMatrix.getElement(rowIndex, colIndex) << ' ';
        os << '\n';  
    }  
    return os;  
}  
  
int main() {  
    SymmetricalMatrix lowerTriangularMatrix{5};
  
    lowerTriangularMatrix.setElement(0, 0, 1);
    lowerTriangularMatrix.setElement(0, 1, 2);
    lowerTriangularMatrix.setElement(0, 2, 3);
    lowerTriangularMatrix.setElement(0, 3, 4);
    lowerTriangularMatrix.setElement(0, 4, 5);
    lowerTriangularMatrix.setElement(1, 1, 6);
    lowerTriangularMatrix.setElement(1, 2, 7);
    lowerTriangularMatrix.setElement(1, 3, 8);
    lowerTriangularMatrix.setElement(1, 4, 9);
    lowerTriangularMatrix.setElement(2, 2, 10);
    lowerTriangularMatrix.setElement(2, 3, 11);
    lowerTriangularMatrix.setElement(2, 4, 12);
    lowerTriangularMatrix.setElement(3, 3, 13);
    lowerTriangularMatrix.setElement(3, 4, 14);
    lowerTriangularMatrix.setElement(4, 4, 15);

    std::cout << lowerTriangularMatrix;  
  
    return 0;  
}

// 1 2 3 4 5
// 2 6 7 8 9
// 3 7 10 11 12
// 4 8 11 13 14
// 5 9 12 14 15
```

## Tridiagonal Matrix

```cpp
class TridiagonalMatrix {
    int* diagonals;
    size_t matrixSize;
public:
    TridiagonalMatrix(const size_t& nBnSize) : 
	    matrixSize(nBnSize), diagonals(new int[3 * nBnSize - 2]) {}

    ~TridiagonalMatrix() {
        delete[] diagonals;
    }

    void setElement(const int& row, const int& column, const int& element) {
        if (std::abs(row - column) <= 1) {
            if (row == column)
                *(diagonals + matrixSize - 1 + row) = element;
            else if (column > row)
                *(diagonals + row) = element;
            else
                *(diagonals + matrixSize * 2 - 1 + column) = element;
        }
    }

    int getElement(const int& row, const int& column) const {
        if (std::abs(row - column) > 1)
            return 0;

        if (row == column)
            return *(diagonals + matrixSize - 1 + row);
        else if (column > row)
            return *(diagonals + row);
        else
            return *(diagonals + matrixSize * 2 - 1 + column);
    }

    friend std::ostream& operator<<(std::ostream& os, const TridiagonalMatrix& tridiagonalMatrix);
};

std::ostream& operator<<(std::ostream& os, const TridiagonalMatrix& tridiagonalMatrix) {
    for (int i = 0; i < tridiagonalMatrix.matrixSize; ++i) {
        for (int j = 0; j < tridiagonalMatrix.matrixSize; ++j)
            os << (std::abs(i - j) <= 1 ? tridiagonalMatrix.getElement(i, j) : 0) << ' ';
        os << '\n';
    }

    return os;
}

int main() {
    TridiagonalMatrix tridiagonalMatrix{5};

    tridiagonalMatrix.setElement(0, 1, 10);
    tridiagonalMatrix.setElement(1, 2, 20);
    tridiagonalMatrix.setElement(2, 3, 30);
    tridiagonalMatrix.setElement(3, 4, 40);

    tridiagonalMatrix.setElement(0, 0, 1);
    tridiagonalMatrix.setElement(1, 1, 2);
    tridiagonalMatrix.setElement(2, 2, 3);
    tridiagonalMatrix.setElement(3, 3, 4);
    tridiagonalMatrix.setElement(4, 4, 5);

    tridiagonalMatrix.setElement(1, 0, 100);
    tridiagonalMatrix.setElement(2, 1, 200);
    tridiagonalMatrix.setElement(3, 2, 300);
    tridiagonalMatrix.setElement(4, 3, 400);

    std::cout << tridiagonalMatrix;

    return 0;
}

// 1   10  0   0   0  
// 100 2   20  0   0  
// 0   200 3   30  0  
// 0   0   300 4   40  
// 0   0   0   400 5
```

## Toeplitz Matrix

```cpp
class ToeplitzMatrix {
    int *uniqueElements;
    size_t matrixSize;

public:
    ToeplitzMatrix(const int &nBnSize) : 
	    matrixSize(nBnSize), uniqueElements(new int[nBnSize * 2 - 1]) {}

    void setElement(const int &row, const int &column, const int &element) {
        if (row == 0) {
            *(this->uniqueElements + column) = element;
            return;
        }

        if (column == 0) {
            *(this->uniqueElements + this->matrixSize - 1 + row) = element;
            return;
        }

        if (column > row)
            *(this->uniqueElements + column + row) = element;
        else
            *(this->uniqueElements + this->matrixSize + row - column) = element;
    }

    int getElement(const int& row, const int& column) const {
        if (row == 0)
            return *(this->uniqueElements + column);
        if (column == 0)
            return *(this->uniqueElements + this->matrixSize - 1 + row);

        if (column < row)
            return *(this->uniqueElements + this->matrixSize - 1 + (row - column));
        else
            return *(this->uniqueElements + column - row);
    }

    friend std::ostream& operator<<(std::ostream& os, const ToeplitzMatrix& toeplitzMatrix);
};

std::ostream& operator<<(std::ostream& os, const ToeplitzMatrix& toeplitzMatrix) {
    for (int i = 0; i < toeplitzMatrix.matrixSize; ++i) {
        for (int j = 0; j < toeplitzMatrix.matrixSize; ++j)
            os << toeplitzMatrix.getElement(i, j) << ' ';
        os << '\n';
    }

    return os;
}

int main() {
    ToeplitzMatrix toeplitzMatrix{5};

    toeplitzMatrix.setElement(0, 0, 2);
    toeplitzMatrix.setElement(0, 1, 3);
    toeplitzMatrix.setElement(0, 2, 4);
    toeplitzMatrix.setElement(0, 3, 5);
    toeplitzMatrix.setElement(0, 4, 6);

    toeplitzMatrix.setElement(1, 0, 7);
    toeplitzMatrix.setElement(2, 0, 8);
    toeplitzMatrix.setElement(3, 0, 9);
    toeplitzMatrix.setElement(4, 0, 10);

    std::cout << toeplitzMatrix;

    return 0;
}

// 2  3  4  5  6  
// 7  2  3  4  5  
// 8  7  2  3  4  
// 9  8  7  2  3  
// 10 9  8  7  2
```

## Sparse Matrix

```cpp
#include <iostream>
#include <sstream>

struct MatrixElement {
    int row;
    int column;
    int element;
};


class SparseMatrix {
    int rows; // Matrix dimensions
    int columns; // Matrix dimensions
    int nonZeroElements; // Number of non-empty values
    MatrixElement **matrixElements; // Dynamic array of the non-empty values
    int currentMatrixElementIndex; // Index for keeping track of the currently added non-empty value

public:
    SparseMatrix(const int &rows, const int &columns, const int &nonZeroElements)
        : rows(rows), columns(columns), nonZeroElements(nonZeroElements),
          matrixElements(new MatrixElement *[nonZeroElements]{}), currentMatrixElementIndex(-1) {}

    SparseMatrix(const SparseMatrix& other) :
        rows(other.rows), columns(other.columns), nonZeroElements(other.nonZeroElements),
        matrixElements(new MatrixElement *[other.nonZeroElements]{}), currentMatrixElementIndex(other.currentMatrixElementIndex) {
        for (int i = 0; i < other.currentMatrixElementIndex + 1; ++i) {
            const MatrixElement* otherCurrentMatrixElement = *(other.matrixElements + i);
            *(this->matrixElements + i) = new MatrixElement{
                otherCurrentMatrixElement->row,
                otherCurrentMatrixElement->column,
                otherCurrentMatrixElement->element};
        }
    }

    ~SparseMatrix() {
        for (int i = 0; i < this->currentMatrixElementIndex + 1; ++i)
            if (this->matrixElements)
                delete this->matrixElements[i];
        delete[] this->matrixElements;
    }

    SparseMatrix& operator=(const SparseMatrix& other) {
        if (this == &other)
            return *this;

        for (int i = 0; i < this->currentMatrixElementIndex + 1; ++i)
            delete this->matrixElements[i];
        delete[] this->matrixElements;

        this->rows = other.rows;
        this->columns = other.columns;
        this->nonZeroElements = other.nonZeroElements;
        this->matrixElements = new MatrixElement *[other.nonZeroElements];
        this->currentMatrixElementIndex = other.currentMatrixElementIndex;

        for (int i = 0; i < other.currentMatrixElementIndex + 1; ++i) {
            const MatrixElement* otherCurrentMatrixElement = *(other.matrixElements + i);
            *(this->matrixElements + i) = new MatrixElement{
                otherCurrentMatrixElement->row,
                otherCurrentMatrixElement->column,
                otherCurrentMatrixElement->element};
        }
        return *this;
    }

    SparseMatrix(SparseMatrix&& other) noexcept // Move constructor
    : rows(other.rows), columns(other.columns), nonZeroElements(other.nonZeroElements),
      matrixElements(other.matrixElements), currentMatrixElementIndex(other.currentMatrixElementIndex) {
        other.matrixElements = nullptr;
        other.currentMatrixElementIndex = -1;
    }

    void addElement(const int &row, const int &column, const int &element) {
        if (this->currentMatrixElementIndex + 1 >= this->nonZeroElements)
            return;
        *(this->matrixElements + (++this->currentMatrixElementIndex)) = new MatrixElement{row, column, element};
    }

    [[nodiscard]] int getElement(const int &row, const int &column) const {
        for (int i = 0; i < this->currentMatrixElementIndex + 1; ++i)
            if (row == this->matrixElements[i]->row && column == this->matrixElements[i]->column)
                return this->matrixElements[i]->element;

        return 0;
    }

    SparseMatrix operator+(const SparseMatrix &other) const {
        // if (other.columns == this->columns && other.rows == this->rows) {
        SparseMatrix resultAddition{
            this->rows,
            this->columns,
            this->nonZeroElements + other.nonZeroElements
        };

        for (int i = 0; i < this->currentMatrixElementIndex + 1; ++i) {
            for (int j = 0; j < other.currentMatrixElementIndex + 1; ++j) {
                if (
                    this->matrixElements[i]->column == other.matrixElements[j]->column &&
                    this->matrixElements[i]->row == other.matrixElements[j]->row
                ) {
                    resultAddition.addElement(
                        this->matrixElements[i]->row,
                        this->matrixElements[i]->column,
                        this->matrixElements[i]->element + other.matrixElements[j]->element
                    );
                    goto elementFound;
                }
            }
            resultAddition.addElement(
                this->matrixElements[i]->row,
                this->matrixElements[i]->column,
                this->matrixElements[i]->element
            );
        elementFound:
        }

        for (int i = 0; i < other.currentMatrixElementIndex + 1; ++i) {
            for (int j = 0; j < this->currentMatrixElementIndex + 1; ++j) {
                if (
                    other.matrixElements[i]->column == this->matrixElements[j]->column &&
                    other.matrixElements[i]->row == this->matrixElements[j]->row
                )
                    goto elementFound2;
            }
            resultAddition.addElement(
                other.matrixElements[i]->row,
                other.matrixElements[i]->column,
                other.matrixElements[i]->element
            );
        elementFound2:
        }

        resultAddition.nonZeroElements = resultAddition.currentMatrixElementIndex + 1;
        return resultAddition;
        // }
    }

    friend std::ostream &operator<<(std::ostream &os, const SparseMatrix &sparseMatrix);
};

std::ostream &operator<<(std::ostream &os, const SparseMatrix &sparseMatrix) {
    for (int i = 0; i < sparseMatrix.rows; ++i) {
        for (int j = 0; j < sparseMatrix.columns; ++j)
            os << sparseMatrix.getElement(i, j) << ' ';
        os << '\n';
    }

    return os;
}


int main() {
    SparseMatrix sparseMatrix{4, 5, 5};
    sparseMatrix.addElement(0, 2, 7);
    sparseMatrix.addElement(1, 0, 2);
    sparseMatrix.addElement(1, 3, 5);
    sparseMatrix.addElement(2, 0, 9);
    sparseMatrix.addElement(3, 4, 4);


    SparseMatrix sparseMatrix2{4, 5, 5};
    sparseMatrix2.addElement(0, 2, 7);
    sparseMatrix2.addElement(1, 0, 2);
    sparseMatrix2.addElement(1, 3, 5);
    sparseMatrix2.addElement(2, 0, 9);
    sparseMatrix2.addElement(3, 4, 4);

    SparseMatrix result = sparseMatrix + sparseMatrix2;
    std::cout << result;

    return 0;
}

```
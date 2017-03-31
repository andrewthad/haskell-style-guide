# Haskell Style Guide

These are some rules I usually follow when writing code in haskell.

## Imports

### Qualified Imports

Use concise acronyms and abbreviations for module import aliases. Always
use the same alias everywhere. Don't import `Data.ByteString.Lazy` as `BL`
in one place and then `LB` in another.
One-letter aliases are scarce; there are only 26 of them!
They are typically given to the most common imports from packages like `base`,
`containers`, `text`, and `bytestring`. In general, prefer acronyms
that capture the way the module name with adjectives shifted to the
front. For example, for `Data.ByteString.Lazy`, the two potential candidates
discussed earlier were `BL` and `LB`. This guide recommends `LB` because
we talk of **L**azy **B**yteStrings. Similarly, the module `Data.Vector.Mutable`
becomes `MV` because it deals with **M**utable **V**ectors.
It's difficult to remember what to use, and this isn't by any means
a clear-cut science, so below is a list to help standardize it:


```haskell
import qualified Data.Semigroup as SG
import qualified Data.Hashable as H
import qualified Data.Text as T
import qualified Data.Text.Lazy as LT
import qualified Data.ByteString as B
import qualified Data.ByteString.Lazy as LB
import qualified Data.ByteString.Char8 as BC
import qualified Data.ByteString.Lazy.Char8 as LBC
import qualified Data.Map.Strict as M
import qualified Data.Sequance as SQ
import qualified Data.Set as S
import qualified Data.Vector as V
import qualified Data.Vector.Mutable as MV
import qualified Data.Vector.Unboxed as UV
import qualified Data.Vector.Unboxed.Mutable as MUV
import qualified Data.HashMap.Strict as HM
import qualified Data.HashSet as HS
import qualified Data.IntMap.Strict as IM
import qualified Pipes as P
import qualified Pipes.Prelude as PP
import qualified Streaming as SM
import qualified Streaming.Prelude as SMP
import qualified Data.Attoparsec.ByteString as AB
import qualified Data.Attoparsec.Text as AT
import qualified Data.Aeson as AE
import qualified Data.Aeson.Types as AET
```

### Augmenting the Prelude

When writing an *application*, create a prelude augmentation module that exports 
types and easily recognized functions from dependencies commonly used in
the project. Do not do this when writing a *library*, since
library source code tends to be read or skimmed by a larger audience, and the
extra verbosity is useful.
It does not need to reexport the prelude since the prelude
is already imported everywhere by default. For example:

```haskell
module MyApp.Prelude
  ( module I
  ) where

import Data.Text as I (Text)
import Data.ByteString as I (ByteString)
import Data.Aeson as I (FromJSON(..),ToJSON(..))
import Data.Vector as I (Vector)
import Data.Vector.Mutable as I (MVector)
import Data.Monoid as I
import Control.Monad as I
import Control.Applicative as I
import Data.Functor.Identity as I (Identity(..))
import Control.Monad.Trans.Reader as I (ReaderT(..))
import Control.Monad.Trans.State.Strict as I (StateT(..),execStateT)
import Data.Semigroup as I (Semigroup)
```

Again, this is just an example. A real project might have five or ten times
as many lines in its prelude augmentation module. To bring this into other modules,
simply add `import MyApp.Prelude` to all other files in the project.
This module should augment the normal prelude from `base`. It
should be entirely compatible with it and should not replace any of the functions
with more general variants. This means that the `NoImplicitPrelude` extension
should **not** be needed, nor should it be neccessary to hide the entire
prelude with `import Prelude ()`.




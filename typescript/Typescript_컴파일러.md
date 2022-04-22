# Typescript 컴파일러

이 글은 타입스크립트를 컴파일할때 어떻게 컴파일할 수 있는지 방법에 대해 기록한다.

## Watch mode

일반적으로 맨처음 typescript를 시작하면 tsc 커맨드를 사용한다. 예를들어 ```tsc app.ts```를 사용한다.    
이 방법은 파일을 변경할때마다 tsc를 커맨드를 사용해야한다. 이 불편함을 없애기 위해 watch 모드를 사용하는데 
```tsc app.ts --watch``` 이렇게 사용하거나 ```tsc app.ts -w``` 이렇게 커맨드 마지막에 --watch을 붙이거나 -w를 붙이면 된다. 
이를 사용하면 타입스크립트가 파일을 관찰하고 파일에 변경사항이 있을때마다 다시 컴파일하게 된다. 

## 전체 프로젝트 파일 / 다수 파일 

그렇다면 전체 프로젝트 파일 혹은 다수 파일을 컴파일 할 경우 어떻게 해야할까?
예를 들어 app.ts그리고 analysctic.ts 두개의 파일이 존재한다고 하면 ```tsc app.ts```입력후 ```tsc analystic.ts``` 입력하면 되기도 한다. 


하지만 파일을 이렇게 지정하지 않아도 관찰모드로 전체 프로젝트 폴더를 확인하고 변경사항이 적용될 수 있는 모든 타입스크립트 파일을 다시 컴파일 할 수 있다. 방법은 다음과 같다 


먼저 이 프로젝트 폴더를 typescript폴더라고 알려주어야 한다. ```tsc --init```을 입력한다. 그러면 ```tsconfig.json```파일이 생성된다.   
이 파일은 타입스크립트가 관리해야 하는 이 파일이 포함된 프로젝트와 이 폴더의 모든 하위 폴더를 참고하기 위한 파일이다.  
그리고 ```tsc```를 입력하게 되면 이 프로젝트 폴더 하위에 있는 ts파일을 컴파일 하게 된다.  
또한 여기에도 ```tsc -w ``` 혹은 ```tsc --watch```를 사용해 watch mode를 사용할 수 있다. 

## 파일 포함 및 제외하기 

우리는 tsconfig.json 파일을 수정해서 특정 파일을 컴파일에 포함시키거나 특정 파이릉 컴파일에 제외시킬 수 있다.   
추가할 수 있는 옵션으로 "include" 과 "exclude" 옵션이 있는다. 
```json
...
  ,
  "exclude" : [ // 여기에 들어가는 파일은 제외한다는 의미이다. 
    "analystic.ts", 
    "**/*.dev.ts", // 별칭을 통해 뒤에 dev.ts 파일들은 제외 시킨다는 의미이다. 
    "node_modules" // 기본적으로 exclude가 없을 시에는 컴파일할때 제외하지만 exclude가 옵션에 들어가게 되면 반드시 들어가야한다. 
  ],
  "include" : [ // 여기에 들어가는 파일만 컴파일한다. 
    "app.ts" // 예를 들어 exclude와 include가 함께 쓰이게 되면 exclude를 제외한 채 include를 컴파일 한다. 
  ]
...
```

## 컴파일 대상 설정하기

```json
{
  "compilerOptions": {
    "target": "es2016", 
   }
}
```
tsconfig.json 파일을 보게 되면 많은 옵션들을 볼 수 있는데 그 중에서 ```"tartget"```을 보게 되면 컴파일하는 javascript 코드 버전을 설정할 수 있다. 
위 예시를 보게되면 es2016으로 컴파일 한다는 의미이다. 이게 의미하는건 예전버전의 자바스크립트로도 컴파일 할 수 있다는 의미이다. 

## Typescript 핵심 라이브러리 이해하기 

```json
{
  "compilerOptions": {
    "target": "es2016", 
    "lib": []
   }
}
```
다음 ```lib```라고 되어 있는 옵션을 보게 되면 여기에 라이브러리를 추가 할 수 있는데, 예를들어 dom API 같은 것들을 추가 할 수 있다. 하지만 실제로는 만질 필요가 없는데 왜냐하면 es2016에 기본적으로 들어가져 있기 때문이다. 추가적으로 일일이 설정하기 위해선 여기 ```lib```옵션을 설정해 주면 된다. 

## 다른 옵션들 

유용한게 많아 한번은 읽어보는게 좋다.
```json
{
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig.json to read more about this file */

    /* Projects */
    // "incremental": true,                              /* Enable incremental compilation */
    // "composite": true,                                /* Enable constraints that allow a TypeScript project to be used with project references. */
    // "tsBuildInfoFile": "./",                          /* Specify the folder for .tsbuildinfo incremental compilation files. */
    // "disableSourceOfProjectReferenceRedirect": true,  /* Disable preferring source files instead of declaration files when referencing composite projects */
    // "disableSolutionSearching": true,                 /* Opt a project out of multi-project reference checking when editing. */
    // "disableReferencedProjectLoad": true,             /* Reduce the number of projects loaded automatically by TypeScript. */

    /* Language and Environment */
    "target": "es2016",                                  /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */
    // "lib": [],                                        /* Specify a set of bundled library declaration files that describe the target runtime environment. */
    // "jsx": "preserve",                                /* Specify what JSX code is generated. */
    // "experimentalDecorators": true,                   /* Enable experimental support for TC39 stage 2 draft decorators. */
    // "emitDecoratorMetadata": true,                    /* Emit design-type metadata for decorated declarations in source files. */
    // "jsxFactory": "",                                 /* Specify the JSX factory function used when targeting React JSX emit, e.g. 'React.createElement' or 'h' */
    // "jsxFragmentFactory": "",                         /* Specify the JSX Fragment reference used for fragments when targeting React JSX emit e.g. 'React.Fragment' or 'Fragment'. */
    // "jsxImportSource": "",                            /* Specify module specifier used to import the JSX factory functions when using `jsx: react-jsx*`.` */
    // "reactNamespace": "",                             /* Specify the object invoked for `createElement`. This only applies when targeting `react` JSX emit. */
    // "noLib": true,                                    /* Disable including any library files, including the default lib.d.ts. */
    // "useDefineForClassFields": true,                  /* Emit ECMAScript-standard-compliant class fields. */

    /* Modules */
    "module": "commonjs",                                /* Specify what module code is generated. */
    // "rootDir": "./",                                  /* Specify the root folder within your source files. */ // 소스의 구체적인 파일루트를 정해줌
    // "moduleResolution": "node",                       /* Specify how TypeScript looks up a file from a given module specifier. */
    // "baseUrl": "./",                                  /* Specify the base directory to resolve non-relative module names. */
    // "paths": {},                                      /* Specify a set of entries that re-map imports to additional lookup locations. */
    // "rootDirs": [],                                   /* Allow multiple folders to be treated as one when resolving modules. */
    // "typeRoots": [],                                  /* Specify multiple folders that act like `./node_modules/@types`. */
    // "types": [],                                      /* Specify type package names to be included without being referenced in a source file. */
    // "allowUmdGlobalAccess": true,                     /* Allow accessing UMD globals from modules. */
    // "resolveJsonModule": true,                        /* Enable importing .json files */
    // "noResolve": true,                                /* Disallow `import`s, `require`s or `<reference>`s from expanding the number of files TypeScript should add to a project. */

    /* JavaScript Support */
    // "allowJs": true,                                  /* Allow JavaScript files to be a part of your program. Use the `checkJS` option to get errors from these files. */
    // "checkJs": true,                                  /* Enable error reporting in type-checked JavaScript files. */
    // "maxNodeModuleJsDepth": 1,                        /* Specify the maximum folder depth used for checking JavaScript files from `node_modules`. Only applicable with `allowJs`. */

    /* Emit */
    // "declaration": true,                              /* Generate .d.ts files from TypeScript and JavaScript files in your project. */
    // "declarationMap": true,                           /* Create sourcemaps for d.ts files. */
    // "emitDeclarationOnly": true,                      /* Only output d.ts files and not JavaScript files. */
    // "sourceMap": true,                                /* Create source map files for emitted JavaScript files. */ // 디버깅시 매우 좋음 웹에서 ts파일 확인가능
    // "outFile": "./",                                  /* Specify a file that bundles all outputs into one JavaScript file. If `declaration` is true, also designates a file that bundles all .d.ts output. */
    // "outDir": "./",                                   /* Specify an output folder for all emitted files. */ // 컴파일시 특정위치에 js파일을 만듬.
    // "removeComments": true,                           /* Disable emitting comments. */
    // "noEmit": true,                                   /* Disable emitting files from a compilation. */
    // "importHelpers": true,                            /* Allow importing helper functions from tslib once per project, instead of including them per-file. */
    // "importsNotUsedAsValues": "remove",               /* Specify emit/checking behavior for imports that are only used for types */
    // "downlevelIteration": true,                       /* Emit more compliant, but verbose and less performant JavaScript for iteration. */
    // "sourceRoot": "",                                 /* Specify the root path for debuggers to find the reference source code. */
    // "mapRoot": "",                                    /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,                          /* Include sourcemap files inside the emitted JavaScript. */
    // "inlineSources": true,                            /* Include source code in the sourcemaps inside the emitted JavaScript. */
    // "emitBOM": true,                                  /* Emit a UTF-8 Byte Order Mark (BOM) in the beginning of output files. */
    // "newLine": "crlf",                                /* Set the newline character for emitting files. */
    // "stripInternal": true,                            /* Disable emitting declarations that have `@internal` in their JSDoc comments. */
    // "noEmitHelpers": true,                            /* Disable generating custom helper functions like `__extends` in compiled output. */
    // "noEmitOnError": true,                            /* Disable emitting files if any type checking errors are reported. */ //에러가 있을시 컴파일 하지 않음
    // "preserveConstEnums": true,                       /* Disable erasing `const enum` declarations in generated code. */
    // "declarationDir": "./",                           /* Specify the output directory for generated declaration files. */
    // "preserveValueImports": true,                     /* Preserve unused imported values in the JavaScript output that would otherwise be removed. */

    /* Interop Constraints */
    // "isolatedModules": true,                          /* Ensure that each file can be safely transpiled without relying on other imports. */
    // "allowSyntheticDefaultImports": true,             /* Allow 'import x from y' when a module doesn't have a default export. */
    "esModuleInterop": true,                             /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables `allowSyntheticDefaultImports` for type compatibility. */
    // "preserveSymlinks": true,                         /* Disable resolving symlinks to their realpath. This correlates to the same flag in node. */
    "forceConsistentCasingInFileNames": true,            /* Ensure that casing is correct in imports. */

    /* Type Checking */
    "strict": true,                                      /* Enable all strict type-checking options. */
    // "noImplicitAny": true,                            /* Enable error reporting for expressions and declarations with an implied `any` type.. */
    // "strictNullChecks": true,                         /* When type checking, take into account `null` and `undefined`. */
    // "strictFunctionTypes": true,                      /* When assigning functions, check to ensure parameters and the return values are subtype-compatible. */
    // "strictBindCallApply": true,                      /* Check that the arguments for `bind`, `call`, and `apply` methods match the original function. */
    // "strictPropertyInitialization": true,             /* Check for class properties that are declared but not set in the constructor. */
    // "noImplicitThis": true,                           /* Enable error reporting when `this` is given the type `any`. */
    // "useUnknownInCatchVariables": true,               /* Type catch clause variables as 'unknown' instead of 'any'. */
    // "alwaysStrict": true,                             /* Ensure 'use strict' is always emitted. */
    // "noUnusedLocals": true,                           /* Enable error reporting when a local variables aren't read. */
    // "noUnusedParameters": true,                       /* Raise an error when a function parameter isn't read */
    // "exactOptionalPropertyTypes": true,               /* Interpret optional property types as written, rather than adding 'undefined'. */
    // "noImplicitReturns": true,                        /* Enable error reporting for codepaths that do not explicitly return in a function. */
    // "noFallthroughCasesInSwitch": true,               /* Enable error reporting for fallthrough cases in switch statements. */
    // "noUncheckedIndexedAccess": true,                 /* Include 'undefined' in index signature results */
    // "noImplicitOverride": true,                       /* Ensure overriding members in derived classes are marked with an override modifier. */
    // "noPropertyAccessFromIndexSignature": true,       /* Enforces using indexed accessors for keys declared using an indexed type */
    // "allowUnusedLabels": true,                        /* Disable error reporting for unused labels. */
    // "allowUnreachableCode": true,                     /* Disable error reporting for unreachable code. */

    /* Completeness */
    // "skipDefaultLibCheck": true,                      /* Skip type checking .d.ts files that are included with TypeScript. */
    "skipLibCheck": true                                 /* Skip type checking all .d.ts files. */
  }
}

```

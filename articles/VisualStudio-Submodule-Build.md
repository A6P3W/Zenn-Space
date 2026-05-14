---
title: ViualStudio
emoji: 💭
type: tech
topics:
  - "1"
  - "2"
  - "3"
  - "4"
published: false
---
git submodule add
追加ー既存のプロジェクト
submoduleに.propsを作成
```
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
	<ItemDefinitionGroup>
		<ClCompile>
			<AdditionalIncludeDirectories>$(DXLIB_DIR);$(MSBuildThisFileDirectory)Engine/EngineCore/;$(MSBuildThisFileDirectory)Engine/EngineCore/Core/Public;$(MSBuildThisFileDirectory)Engine/EngineCore/Systems/Public;$(MSBuildThisFileDirectory)Engine/EngineCore/Components/Public;$(MSBuildThisFileDirectory)Engine/EngineSide/;$(MSBuildThisFileDirectory)Engine/EngineCore/GameFramework/Public;%(AdditionalIncludeDirectories)</AdditionalIncludeDirectories>
		</ClCompile>
		<Link>
			<AdditionalDependencies>VS-DxLib-Learn-01.lib;%(AdditionalDependencies)</AdditionalDependencies>
			<AdditionalLibraryDirectories>$(SolutionDir)$(Platform)\$(Configuration)\;$(SolutionDir)BroccoliEngine\$(Platform)\$(Configuration)\;%(AdditionalLibraryDirectories)</AdditionalLibraryDirectories>
		</Link>
	</ItemDefinitionGroup>
</Project>
```
プロパティシートー既存の追加ー追加
プロジェクト ビルドの依存関係　submoduleを追加
スタートアッププロジェクトの設定

submoduleは.libを生成


$(MSBuildThisFileDirectory)


propsのインクルードパスを認識させるため
%(AdditionalLibraryDirectories);